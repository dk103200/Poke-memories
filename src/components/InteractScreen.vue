<template>
<div class="screen">
  <div class="screen__inner"
    :style="{  width: `${(((920 - 16 * 4) / Math.sqrt(cardContexts.length) - 16) *3 / 4 +
    16) * Math.sqrt(cardContexts.length)}px`,}"
  >

    <card-flip v-for="(card,index) in cardContexts"
    :key="index" 
    :ref="`card-${index}`"
    :imgBFUrl="`images/${card}.png`"
    :card="{ index, value:card}"
    :cardContexts="cardContexts"
    :rules="rules"
    @onFlip="checkRule($event)"
  />
  </div>
  
</div>
</template>

<script>
import CardFlip from './Card.vue'
export default {
  props:{
    cardContexts: {
      type: Array,
      default: function(){
        return []
      }
    },
  },
  components: {
    CardFlip
  },data() {
    return {
      rules: [],
    }
  },
  methods: {
    checkRule(card){
     if (this.rules.length === 2 ) return false
     this.rules.push(card)

      if (this.rules.length === 2 && 
      this.rules[0].value === this.rules[1].value){
      
          console.log("True")
            this.$refs[`card-${this.rules[0].index}`].onDisableMode()
            this.$refs[`card-${this.rules[1].index}`].onDisableMode()

          this.rules = []
          const disElements = document.querySelectorAll(".screen .card.disable")
          if (disElements && 
          disElements.length === this.cardContexts.length - 2){
            setTimeout(()=>{
              this.$emit("onFinish")

            },920)
          }
        }else if (this.rules.length === 2 && 
        this.rules[0].value !== this.rules[1].value){
          console.log("Wrong")
          setTimeout(()=>{
            this.$refs[`card-${this.rules[0].index}`].onFlipBack()
            this.$refs[`card-${this.rules[1].index}`].onFlipBack()

            this.rules = []
          },800)
      } else return false
      
    }
  },
}
</script>

<style scoped>

  .screen {
    width: 100%;
    height: 100vh;
    position: absolute;
    text-align: center;
    top: 0;
    left: 0;
    z-index: 2;
    background-color: var(--dark);
    color: var(--light);
  }
  .screen__inner {
    display: flex;    
    flex-wrap: wrap;
    margin: 2rem auto;
  }
  
</style>

<template>
  <div class="card" :class="{'disable': isDisabled}"
    :style="{
      height: `${(920 - 16 * 4) / Math.sqrt(cardContexts.length) - 16}px`,
      width: `${(((920 - 16 * 4) / Math.sqrt(cardContexts.length) - 16) *3)/4}px`,
      perpective:`${ (((920 - 16 * 4) / Math.sqrt(cardContexts.length) - 16) *3)/2}px`,
      }">
    <div class="card__inner" 
    :class="{'is-flipped': isFlipped}"
    @click="onToggleFlipCard()"
    >
      <div class="card__face card__face--font">
        <div class="card__content"
          :style="{
            backgroundSize: `${((920 - 16 * 4) / Math.sqrt(cardContexts.length) - 16) / 4 }px`
          }"
        ></div>
      </div>
      <div class="card__face card__face--back">
        <div class="card__content" :style="{backgroundImage: `url(${require('@/assets/' + imgBFUrl)})`}"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props :{
    card:{
      type:[String, Number, Array, Object]
    },
    imgBFUrl: {
      type: String,
      required:true,
    },
    cardContexts: {
      type: Array,
      default: function(){
        return []
      }
    }
  },
  data(){
    return {
      isDisabled: false,
      isFlipped: false
    }
  },
  methods: {
    onToggleFlipCard(){
      if (this.isDisabled) return false
      this.isFlipped = !this.isFlipped
      if (this.isFlipped) this.$emit("onFlip", this.card)
    }, onFlipBack(){
      this.isFlipped = false
    },
    onDisableMode() {
      this.isDisabled = true
    }
  },
}
</script>

<style lang="css" scoped>
  .card {
    display: inline-block;
    margin-right: 1rem;
    margin-bottom: 1rem;
   }
  .card__inner {
    width: 100%;
    height: 100%;
    transition: transform 0.8s;
    transform-style: preserve-3d;
    cursor: pointer;
    position: relative;
  }
  .card.disable .card__inner {
    cursor: default;
  }

  .card__inner.is-flipped {
    transform: rotateY(-180deg);
  }

  .card__face {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    overflow: hidden;
    border-radius: 1rem;
    padding: 1rem;
    box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
  }

  .card__face--font .card__content {
    background: url('../assets/images/icon_back.png') no-repeat center center;
    height: 100%;
    background-size: 40px 40px;
    width: 100%;
  }
  .card__face--back {
    background-color: var(--light);
    transform: rotateY(-180deg);
  }

  .card__face--back .card__content{
    background-size: contain;
    background-position: center center;
    background-repeat: no-repeat;
    height: 100%;
    width: 100%;
  }
</style>

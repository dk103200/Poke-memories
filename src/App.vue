<template>
<div>
<main-screen v-if="statusMatch === 'default'" @onStart="onHandleStart($event)"></main-screen>

<interact-screen 
  v-if="statusMatch === 'match'" 
  :cardContexts="settings.cardContexts"
  @onFinish="onGetResult()"
>
</interact-screen>

<result-screen :timer="timer" 
  v-if="statusMatch === 'result'" 
  @onStartAgain="statusMatch = 'default'"
/>
<copy-right></copy-right>
</div>

</template> 

<script>
import MainScreen from './components/MainScreen.vue'
import InteractScreen from './components/InteractScreen.vue'
import ResultScreen from './components/ResultScreen.vue'
import CopyRight from './components/CopyRight.vue'

import { shuffled } from './utils/array'

export default {
  name: 'App', 
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CopyRight
  },
  data(){
    return {
      settings:{
        totalBlocks: 0,
        cardContexts: [],
        startedAt: null
      },
      statusMatch: "default",
      timer:  0,
    }
  },
 
  methods: {
      onHandleStart(config){
        console.log('running handle start ', config)
        this.settings.totalBlocks = config.totalBlocks

        const firstCards = Array.from({length: this.settings.totalBlocks /2},
          (_,i)=> i+1
          )
        const cards = [...firstCards, ...firstCards] 
        this.settings.cardContexts = shuffled(
          shuffled(shuffled(shuffled(cards))))

        this.settings.startedAt = new Date().getTime()
        this.statusMatch = 'match'
      },
      onGetResult(){
        this.timer = new Date().getTime() - this.settings.startedAt
        this.statusMatch = "result"
      }
  },
}
</script>

<style>

</style>

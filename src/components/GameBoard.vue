<template>
  <div class="game-board">
      <button-pad
        :isRunning="isRunning"
        :originalSequnce="sequence"
        :option="option"
        ref="buttonpad"
        @stop-game="stopGame" @next-round="nextRound"></button-pad>
      <control-panel
        v-model:message="message"
        v-model:isRunning="isRunning"
        v-model:option="option"
        v-model:round="round"></control-panel>
  </div>
</template>

<script>
import ButtonPad from './ButtonPad.vue'
import ControlPanel from './ControlPanel.vue'
export default {
  name: 'GameBoard',
  components: {
    ButtonPad,
    ControlPanel
  },
  data(){
      return{
          isRunning: false,
          option: null,
          round: 0,
          sequence: [],
          message: ''
      }
  },
  watch: {
      round(newRoundValue){
          if (this.isRunning){
            this.sequence = this.generateButtonSequnce(newRoundValue)
            this.$refs.buttonpad.playSequence(this.sequence, this.speed, this.option)
          }
      }
  },
  methods: {
      generateButtonSequnce(size){
          const result = []
          for (let i = 0; i < size; i++){
              result.push(Math.floor(Math.random() * 4) + 1)
          }
          return result 
      },
      stopGame(){
          this.round = 0
          this.isRunning = false
          this.message = 'Sorry you lost'
      },
      nextRound(){
          this.round += 1
      }
  },
  computed:{
      speed(){
          if (this.option == 'medium'){
              return 1000
          }
          else if (this.option == 'hard'){
              return 400
          }
          else {
              return 1500
          }
      }
  }
}

</script>

<style scoped>
.game-board{
    display: flex;
    width: 100%;
    max-width: 540px;
    margin-top: 64px;
}
</style>
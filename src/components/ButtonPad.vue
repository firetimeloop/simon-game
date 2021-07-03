<template>
  <ul class="pad">
      <li v-for="button in buttons"
        class="pad__button"
        :class="[button.classObject, isRunning ? 'pad__button_state_active' : '']" 
        :key="button.id"
        @click="pushToCurrentSequence(button.id)" ></li>
  </ul>
</template>

<script>
export default {
  name: 'ButtonPad',
  props: {
      isRunning: Boolean,
      originalSequnce: Array,
      option: String
  },
  emits: ['stop-game', 'next-round'],
  data(){
      return {
          buttons: [
              {id:1, classObject:{'pad__button_color_blue':true, 'pad__button_state_pushed':false}, colorClass:'pad__button_color_blue'},
              {id:2, classObject:{'pad__button_color_red':true, 'pad__button_state_pushed':false}, colorClass:'pad__button_color_red'},
              {id:3, classObject:{'pad__button_color_yellow':true, 'pad__button_state_pushed':false}, colorClass:'pad__button_color_yellow'},
              {id:4, classObject:{'pad__button_color_green':true, 'pad__button_state_pushed':false}, colorClass:'pad__button_color_green'},
            ],
            currentSequence: []
      }
  },
  watch:{
      currentSequence:{
          handler(newSequnce){
            for (let i = 0; i < this.currentSequence.length; i++){
              if(this.originalSequnce[i] !== newSequnce[i]){
                  this.currentSequence = []
                  if (this.option !== 'free'){
                    this.$emit('stop-game')
                  }
                  return
              }
              if(i === this.originalSequnce.length - 1){
                  this.currentSequence = []
                  this.$emit('next-round')
              }
            }
          },
          deep: true
      }
  },
  methods:{
      playSound(track){
          if (this.isRunning){
            const audio = new Audio(require(`../sounds/${track}.ogg`))
            audio.play()
          }
      },
      toggleButtonState(id) {
          const button = this.buttons.filter(button => button.id === id)[0]
          button.classObject.pad__button_state_pushed = !button.classObject.pad__button_state_pushed
      },
      buttonLight(id, duration){
          this.toggleButtonState(id)
          setTimeout(this.toggleButtonState, duration, id)
      },
      playSequence(sequence, duration, option){
          let i = 0
          const buttonLight = this.buttonLight.bind(this)
          const playSound = this.playSound.bind(this)
          let timer = setInterval(function() {
            if (option !== 'light'){
                playSound(sequence[i])
            }
            if (option !== 'sound'){
                buttonLight(sequence[i], 500)
            }
            i++
            if (i === sequence.length){
                clearInterval(timer)
            }
          }, duration)
      },
      pushToCurrentSequence(id){
          if (this.isRunning){
            if (this.option !== 'light'){
                this.playSound(id)
            }
            this.currentSequence.push(id)
          }
      }
  }
}
</script>

<style scoped>
.pad{
    margin: 0;
    margin-right: 48px;
    padding: 0;
    display: grid;
    grid-template: 1fr 1fr / 1fr 1fr;
    height: 295px;
    max-height: 295px;
    width: 60%;
    max-width: 302px;
    list-style: none;
    border-radius:50%;
    box-shadow: 5px 4px 0px 3px #fff, 5px 4px 12px 5px #aaa;   
}

@media(max-width: 500px){
    .pad{
        box-shadow: none;
        height: 60vw;
        margin-right: 10vw;
    }
}

.pad__button{
    max-width: 296px;
}

@media(max-width: 500px){
    .pad__button{
        height: 30vw;
    }
}

.pad__button:hover{
    cursor: pointer;
}

.pad__button_color_red{
    background-color: rgba(255, 86, 67, 0.6);
    border-radius: 0 150px 0 0;
}

.pad__button_color_red:hover{
    border-top: 2px solid rgb(102, 102, 102);
    border-right: 2px solid rgb(102, 102, 102);
}

.pad__button_color_red.pad__button_state_active:active{
    background-color: #ff5643;
}

.pad__button_color_red.pad__button_state_pushed{
    background-color: #ff5643;
}

.pad__button_color_blue{
    background-color: rgba(30, 144, 255, 0.6);
    border-radius: 150px 0 0 0;
}

.pad__button_color_blue:hover{
    border-top: 2px solid rgb(102, 102, 102);
    border-left: 2px solid rgb(102, 102, 102);
}

.pad__button_color_blue.pad__button_state_active:active{
    background-color: #1e90ff;
}

.pad__button_color_blue.pad__button_state_pushed{
    background-color: #1e90ff;
}

.pad__button_color_yellow{
    background-color: rgba(254, 239, 51, 0.6);
    border-radius: 0 0 0 150px;
}

.pad__button_color_yellow:hover{
    border-bottom: 2px solid rgb(102, 102, 102);
    border-left: 2px solid rgb(102, 102, 102);
}

.pad__button_color_yellow.pad__button_state_active:active{
    background-color: #feef33;
}

.pad__button_color_yellow.pad__button_state_pushed{
    background-color: #feef33;
}

.pad__button_color_green{
    background-color: rgba(190, 222, 21, 0.6);
    border-radius: 0 0 150px 0;
}

.pad__button_color_green:hover{
    border-bottom: 2px solid rgb(102, 102, 102);
    border-right: 2px solid rgb(102, 102, 102);
}

.pad__button_color_green.pad__button_state_active:active{
    background-color: #bede15;
}

.pad__button_color_green.pad__button_state_pushed{
    background-color: #bede15;
}

</style>
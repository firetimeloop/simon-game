<template>
  <form class="control-panel">
    <h2>Round: {{ round }}</h2>
    <button class="control-panel__start-button" @click.prevent="startGame">Start</button>
    <p>{{ message }}</p>
    <h2 class="control-panel__header_bottomless">Game Options:</h2>
    
    <label for="option1" class="control-panel__choice-label">
        <input type="radio" name="group1" id="option1" value="easy" @input="$emit('update:option', $event.target.value)" />
        Easy
    </label>
    <label for="option2" class="control-panel__choice-label">
        <input type="radio" name="group1" id="option2" value="medium" @input="$emit('update:option', $event.target.value)" />
        Medium
    </label>
    <label for="option3" class="control-panel__choice-label">
        <input type="radio" name="group1" id="option3" value="hard" @input="$emit('update:option', $event.target.value)" />
        Hard
    </label>
        <label for="option4" class="control-panel__choice-label">
        <input type="radio" name="group1" id="option4" value="sound" @input="$emit('update:option', $event.target.value)" />
        Sound&nbsp;only
    </label>
        <label for="option5" class="control-panel__choice-label">
        <input type="radio" name="group1" id="option5" value="light" @input="$emit('update:option', $event.target.value)" />
        Light&nbsp;only
    </label>
        <label for="option6" class="control-panel__choice-label">
        <input type="radio" name="group1" id="option6" value="free" @input="$emit('update:option', $event.target.value)" />
        Free&nbsp;board
    </label>
  </form>
</template>

<script>
export default {
  name: 'ControlPanel',
  props: {
      isRunning: Boolean,
      option: String,
      round: Number,
      message: String
  },
  emits: ['update:isRunning', 'update:option'],
  methods: {
      startGame(){
        this.$emit('update:message', '')
        if (this.option !== "free") {
            this.$emit('update:round', 1)
        }
        this.$emit('update:isRunning', !this.isRunning)
      }
  }
}
</script>

<style scoped>
.control-panel{
    display: flex;
    flex-direction: column;
    align-items: flex-start;
}

.control-panel__start-button{
    width: 5em;
    border-radius: 10px 10px 10px 10px;
    background: #6DABE8;
    padding: 0.3em 0.6em;
    border: none;
    font: 1.4em sans-serif;
}

.control-panel__start-button:hover{
    background: #78bcff;
    cursor: pointer;
}

.control-panel__choice-button{
    margin: 3px 10px 0 5px;
}

.control-panel__choice-label{
    display: inline;
}

.control-panel__header_bottomless{
    margin-bottom: 0;
    text-align: left;
}
</style>
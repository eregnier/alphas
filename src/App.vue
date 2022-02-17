<template>
  <div id="app">
    <div v-if="options" class="actions">
      <div class="left">
        <textarea @input="check" cols="40" rows="10" v-model="text"/>
      </div>
      <div class="right">
        <div class="actions-wrapper">
          <button @click="sizeUp">+</button>
          <br/>
          <button @click="sizeDown">-</button>
          <br/>
          <button @click="toggleHelp">?</button>
        </div>
      </div>
      <div class="help" v-if="help">
        <p>Vous pouvez imprimer la page en faisant CTRL+P ou depuis le menu du navigateur</p>
        <p>Pour un meilleur rendu, cachez les options à l'aide du bouton ⚙</p>
        <p>Une remarque, une suggestion, un mot doux ? <a href="https://eregnier.github.io/">contactez-moi !</a></p>
      </div>
    </div>
    <button class="option-toggle" @click="toggleOptions">⚙</button>
    <div class="alphas-wrapper">
      <div :style="styleWrapper" :class="{'clear': token === '\n', 'letter': token !== '\n'}" class="alpha-wrapper"
           v-for="(token, i) in text"
           :key="i">
        <img :style="style" :src="`/alphas/sm-${token}.png`" v-if="isAlpha(token)" :alt="`lettre ${token}`"/>
        <span v-else>{{ token }}</span>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  data() {
    return {
      text: 'super magic',
      height: 100,
      options: true,
      help: false
    }
  },
  methods: {
    toggleHelp() {
      this.help = !this.help
    },
    toggleOptions() {
      this.options = !this.options
    },
    sizeDown() {
      this.height -= 10;
    },
    sizeUp() {
      this.height += 10;
    },
    isAlpha(value) {
      return 'abcdefghijklmnopqrstuvwxyz'.includes(value)
    },
    check() {
      this.text = Array.from(this.text.toLowerCase()).filter(t =>
          this.isAlpha(t) || t === " " || t === "\n"
      ).join('')
    }
  },
  computed: {
    style() {
      return {'height': this.height + 'px'}
    },
    styleWrapper() {
      return {...this.style, 'width': this.height + 'px'}
    }
  }
}
</script>

<style>
* {
  font-family: sans-serif;
  color: #555;
}

.alphas-wrapper {
  width: 100%;
  clear: both;
  padding-top: 15px;
}

.alpha-wrapper {
  float: left;
  margin-left: 3px;
  text-align: center;
}


.clear {
  clear: both;
  width: 0 !important;
}

button {
  background-color: white;
  border: 1px solid #555;
  margin: 4px;
  min-width: 40px;
  min-height: 40px;
  color: #555;
  border-radius: 4px;
  font-size: 2em;
}

.left {
  width: 85%;
  float: left;
  height: 100%;
}

.right {
  width: 15%;
  float: left;
  height: 100%;

}

.actions {
  clear: both;
  width: 100%;
  border-bottom: 2px solid #bbb;
  min-height: 200px;


}

textarea {
  width: 100%;
  height: 90%;
}

.actions-wrapper {
  padding-left: 15px;
}


@media print {
  .option-toggle {
    display: none !important;
  }
}

.option-toggle {
  position: absolute;
  right: 15px;
  top: 15px;
}

.help {
  clear: both;
}
</style>

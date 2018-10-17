<template>
  <div class="complete">
    <textarea
      v-model="text"
      @input="onChange"
      placeholder="Введите ваш текст"
      @keydown.enter="onEnter"
      @keydown.down="onArrowDown"
      @keydown.up="onArrowUp"
      type="text"
    />
    <ul class="points" v-if="isShown">
      <li class="point"
          v-for="(item, i) in data"
          :key="i"
          @click="setValue(item)"
          :class="{ 'active': i === arrowCounter }"
          >
        {{item}}
      </li>
    </ul>
  </div>
</template>

<script>
  import service from '../assets/data';

  export default {
    name: 'complete',
    data() {
      return {
        data: [],
        isShown: false,
        text: '',
        arrowCounter: 0,
        caretPosition: -1
      };
    },
    methods: {
      onChange(event) {
        this.caretPosition = document.querySelector('textarea').selectionStart;
        localStorage.setItem('text', this.text);
        if (this.text.charAt(this.caretPosition - 1) === '{' && this.text.charAt(this.caretPosition - 2) === '{') {
          this.isShown = true;
        }
      },
      setValue (item) {
        this.isShown = false;
        this.text = this.text.slice(0, this.caretPosition) + item + '}}' + this.text.slice(this.caretPosition);
        localStorage.setItem('text', this.text);
        document.querySelector('textarea').focus();
      },
      onArrowDown() {
        if (this.isShown && this.arrowCounter < this.data.length) {
          this.arrowCounter = this.arrowCounter + 1;
        }
      },
      onArrowUp() {
        if (this.isShown && this.arrowCounter > 0) {
          this.arrowCounter = this.arrowCounter - 1;
        }
      },
      onEnter(event) {
        if (this.isShown) {
          event.preventDefault();
          this.setValue(this.data[this.arrowCounter]);
          this.isShown = false;
          this.arrowCounter = 0;
        }
      },
    },
    mounted () {
      const data = service.getData();
      Object.keys(data).forEach((key) => {
        this.data = [...this.data, ...data[key]];
      });
      let text = localStorage.getItem('text');
      if (text) {
        this.text = text;
      }
    }
  };
</script>

<style>
  textarea {
    width: 400px;
    min-height: 150px;
    max-height: 250px;
    font-size: 20px;
    resize: vertical;
    outline: 1px solid lightgreen;
  }


  ul {
    width: 200px;
    background: #cccccc;
    margin: 0 auto;
    border-radius: 5px;
    list-style: none;
    padding-left: 0px;
  }

  li, option {
    border-bottom: 1px solid gray;
    padding: 3px 0;
  }
  li:last-child {
    border-bottom: none;
  }
  li:hover, .active {
    background-color: aquamarine;
  }
</style>

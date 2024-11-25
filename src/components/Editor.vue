<template>
  <div class="editor-container">
    <div class="toolbar">
      <button @click="handleUndo"><i class="fas fa-undo"><img src="../assets/Group 1.svg" alt=""></i></button>
      <button @click="handleRedo"><i class="fas fa-redo"><img src="../assets/Group 2.svg" alt=""></i></button>
      <button @click="formatTextH('h1')"><i class="fas fa-heading"><img src="../assets/Group 3.svg" alt=""></i></button>
      <button @click="formatTextP('p')"><i class="fas fa-paragraph"><img src="../assets/Group 5.svg" alt=""></i></button>
      <button @click="handleImageUpload"><i class="fas fa-image"><img src="../assets/Group 4.svg" alt=""></i></button>
      <button @click="copyHTML"><i class="fas fa-code">Скопировать HTML</i></button>
    </div>
    <div
      class="editor-content"
      contenteditable="true"
      ref="editor"
      @input="handleInput"
    ></div>
  </div>
</template>

<script>
export default {
  name: 'TheEditor',
  data() {
    return {
      history: [],
      currentIndex: -1,
      content: ''
    }
  },
  methods: {
    handleInput(e) {
      this.content = e.target.innerHTML
      this.saveHistory()
    },
    handleUndo() {
      if (this.currentIndex > 0) {
        this.currentIndex--
        this.$refs.editor.innerHTML = this.history[this.currentIndex]
      }
    },
    handleRedo() {
      if (this.currentIndex < this.history.length - 1) {
        this.currentIndex++
        this.$refs.editor.innerHTML = this.history[this.currentIndex]
      }
    },
    formatTextH(type) {
      document.execCommand(type === 'h1' ? 'formatBlock' : type, false, type === 'h1' ? 'h1' : null)
    },
    formatTextP(type){
      document.execCommand(type === 'p' ? 'formatBlock' : type, false, type === 'p' ? 'p' : null)
    },
    handleImageUpload() {
      const input = document.createElement('input')
      input.type = 'file'
      input.accept = 'image/*'
      
      input.onchange = (e) => {
        const file = e.target.files[0]
        const reader = new FileReader()

        reader.onload = (e) => {
          document.execCommand('insertImage', false, e.target.result)
        }
        reader.onload = (event) => {
          const img = document.createElement('img');
          img.src = event.target.result;
          img.style.maxWidth = '680px';
          img.style.height = 'auto';
          
          const selection = window.getSelection();
          const range = selection.getRangeAt(0);
          range.insertNode(img);
        };
        reader.readAsDataURL(file)

      }
      input.click()
    },
    copyHTML() {
      navigator.clipboard.writeText(this.$refs.editor.innerHTML)
    },
    saveHistory() {
      this.currentIndex++
      this.history = this.history.slice(0, this.currentIndex)
      this.history.push(this.content)
      console.log(this.history)
    }
  }
}
</script>

<style scoped>
   button{
    border: none;
    background: none;
  }
  .editor-content img {
    max-width: 100%;
    height: auto;   
    margin: 10px 0;  
  }
  .fa-code{
    color: #639EFF;
    font-family: Roboto;
    font-weight: 400;
    font-style: normal;
    font-size: 15px;
  }
  .editor-container {
    width: 100%;
    max-width: 800px;
    margin: 0 auto;
  }

  .toolbar {
    padding: 10px;
    border-bottom: none;
    background: #1E1E1E;
    display: flex;
  }

  .toolbar button {
    margin: 0 5px;
    padding: 5px 10px;
    cursor: pointer;
  }

  .editor-content {
    min-height: 300px;
    line-height: 1.6;
    padding: 20px;
    background: #1E1E1E;
    position: absolute;
    max-width: 50vw;
  }
  .editor-content:focus {
    outline: none;
  }
</style>

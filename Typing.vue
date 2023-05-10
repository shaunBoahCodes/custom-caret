<template>
  <div id="wrapper" ref="wrapper" @click.self="reset">
    <span id="typer" ref="typer" contenteditable @input="typeIt" @keydown="moveIt" @click="select" spellcheck="false"></span> 
    <p id="display" ref="display" >{{ txt }}</p>
  </div>
</template>



<script setup>
import { ref, onMounted } from 'vue'

//------template refs------\\
const typer = ref(null)
const display = ref(null)

//------data------\\
var txt = ref('')
var caretPos = 0

function incCaretPos() { if (caretPos < typer.value.innerText.length) caretPos++ }
function decCaretPos() { if (caretPos > 0) caretPos-- }

function reset() {
  typer.focus
  caretPos = typer.value.innerText.length
  txt.value = typer.value.innerText
    console.log(typer.value.innerText)
    console.log(txt.value)
}

function typeIt() {
  incCaretPos()
  txt.value = typer.value.innerText.substring(0, caretPos)

  //------handles wrapping text------\\
  const typerHeight = typer.value.offsetHeight
  const fontSize = parseInt(window.getComputedStyle(typer.value).getPropertyValue('font-size'))
  var x = Math.floor(typerHeight / fontSize) - 1
  var dispOffset = -(2 * fontSize) - (x * fontSize) + 'px'
  document.querySelector(':root').style.setProperty('--display-offset', dispOffset)
}

function moveIt(e) {
  if (e.keyCode == 37) { //------left arrow
    decCaretPos()
      console.log(caretPos)
    txt.value = display.value.innerText.substring(0, display.value.innerText.length - 1)
  }
  if (e.keyCode == 39) { //-----right arrow
    incCaretPos()
      console.log(caretPos)
    txt.value = typer.value.innerText.substring(0, caretPos)
  }
  if (e.keyCode == 8) { //------backspace
    decCaretPos()
    if (selected.value) {
      caretPos = selection.anchorOffset - 1 //------handles selection deletion
      txt.value = typer.value.innerText.substring(0, caretPos)
    }
  }
}
var selection = window.getSelection()
var selected = ref(false)
function select() {
  caretPos = selection.focusOffset //------update caretPos
  if (caretPos !== selection.anchorOffset) selected.value = true //------selected range
  txt.value = typer.value.innerText.substring(0, caretPos) //------update txt
 
    console.log(selection.focusNode.data[selection.focusOffset]) //------shows which char was selected
    console.log(caretPos)

  console.log(selected.value)
    
}

//------props------\\
const props = defineProps([
  'fSize', 'tColor', 'cHeight', 'cWidth', 'bTop', 'bRight', 'bBottom', 'bLeft', 'bRadius'
])
const root = document.querySelector(':root')
onMounted(()=> {
  if (props.fSize) root.style.setProperty('--font-size', props.fSize)
  if (props.tColor) root.style.setProperty('--txt-color', props.tColor)
  if (props.cHeight) root.style.setProperty('--height-c', props.cHeight)
  if (props.cWidth) root.style.setProperty('--width-c', props.cWidth)
  if (props.bTop) root.style.setProperty('--border-top', props.bTop)
  if (props.bRight) root.style.setProperty('--border-right', props.bRight)
  if (props.bBottom) root.style.setProperty('--border-bottom', props.bBottom)
  if (props.bLeft) root.style.setProperty('--border-left', props.bLeft)
  if (props.bRadius) root.style.setProperty('--border-radius', props.bRadius)
  
})
</script>



<style>
:root {
  --font-size: 24px;
  --txt-color: rgb(207,207,207);
  --display-offset: -48px;

  --width-c: 11px;
  --height-c: 18px;
  --color-c: ;
  --content-c: ;

  --border-top: 0px solid transparent;
  --border-right: 0px solid transparent;
  --border-bottom: 0px solid transparent;
  --border-left: 0px solid transparent;
  --border-radius: 0px 0px 0px 0px
}
#typer {
  font-size: var(--font-size);
  color: var(--txt-color);
}
#typer:focus {
  outline: none; 
  caret-color: transparent;
}
#display {
  font-size: var(--font-size);
  outline: none;
  color: transparent;
  position: relative;
  top: var(--display-offset);
  z-index: -1000;
  overflow-wrap: break-word;
}
#display::after{
  content: '';
  display: none;

  background: var(--txt-color);
  height: var(--height-c);
  width: var(--width-c);
  border-radius: var(--border-radius);

  position: relative;
  top: 0.5px;
  animation: blink 1s infinite;
}
@keyframes blink {
  30% { opacity: 1;}
  31% { opacity: 0;}
  69% { opacity: 0;}
  70% { opacity: 1;}
}
#typer:focus + #display::after { /* CARET */
  display: inline-block;
}
#wrappper {
  width: 100%;
  height: var(--font-size);
  z-index: -1000;
}
</style>
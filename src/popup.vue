<template>
  <h2 class="title">xiuxiu</h2>
  <div class="labelrow">
    <label for="colorPicker" :style="{ color: enColor }">English Font Color</label>
    <input type="color" id="cncolor" name="colorPicker" v-model="enColor" @change="changeEnColor">
  </div>
  <div class="labelrow">
    <label for="colorPicker" :style="{ color: cnColor }">中文字体颜色</label>
    <input type="color" id="cncolor" name="colorPicker" v-model="cnColor" @change="changeCnColor">
  </div>
  <div class="footer">
    <!-- <a href="https://docs.plasmo.com" target="_blank"> View Docs </a> -->
    <a href="https://github.com/gaao/xiuxiu" target="_blank" class="b"> xiuxiu </a>
    <a href="https://github.com/gaao" target="_blank"> power by gaao </a>
  </div>
</template>

<script setup lang="ts">
import { reactive, ref } from "vue"
import type { App } from "vue"

const state = reactive({ count: 0, action: null })
const enColor = ref('#ff0000')
const cnColor = ref('#0000ff')
function changeEnColor() { }
async function changeCnColor() {
  console.log(222, cnColor.value);
  const tab = await chrome.tabs.query({ active: true, lastFocusedWindow: true });
  console.log('tab信息', tab);
  const a = await chrome.scripting.executeScript({
    // target:{
    //   tabId: tab.id,
    // },
    world:"MAIN",
  })
  // const response = await chrome.tabs.sendMessage(tab.id, {greeting: "hello"});
  // console.log('fasong信息',response);
  // chrome.tabs.query(active:true,currentWidndwo:true),
  // chrome.tabs.sendMessage(tabs{0}.id,{action: "changeCnColor"},function(response) {

  // })
  // chrome.runtime.messaging
  // window.kz_wm.user
}

function increment() {
  state.count++
  state.action = "increment"
}

function decrement() {
  state.count--
  state.action = "decrement"
}

defineOptions({
  prepare(app: App) {
    // Use any plugins here:
    // app.use
  }
})
</script>

<style>
.title {
  text-align: center;
  line-height: 2em;
  padding: 0 0 8px 0;
  border-bottom: 1px solid #eee;
}

.labelrow {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 6px;
  margin-bottom: 8px;
}

.labelrow input {
  cursor: pointer;
}

.footer {
  background-color: rgb(242, 243, 246);
  border-radius: 0.2em;
  min-width: 200px;
  padding: 4px 8px;
  display: flex;
  align-items: center;
  gap: 8px;
}

.b {
  font-weight: bold;
}

.footer a {
  text-decoration: none;
  color: #bbb;
}

.footer a:hover {
  color: #888;
}</style>

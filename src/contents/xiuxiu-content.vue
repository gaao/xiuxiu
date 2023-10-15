<template>
  <div>
    <button @click="changeCharacter" title="(“”‘’：，；;)">替换中英文符号</button>
    <button @click="clg">clg</button>
  </div>
</template>
<script setup lang="ts">
import { reactive, ref } from "vue"
import type { App } from "vue"
import type { PlasmoCSConfig } from "plasmo"
// import popup from "~popup.vue";
import popup from "~popup.vue";
function clg() {
  console.log(111,popup);

}
const config: PlasmoCSConfig = {
  matches: ["*://xiumi.us/studio/*"],
}
function changeCharacter() {
  // const reg = /[u4e00-u9fa5]/
  // function isAllChineseReg(str) {
  //   return /^[\u4E00-\u9FA5]+$/.test(str)
  // }
  function isAllEnglish(str: string) {
    const strLength = str.length;
    let trueCount = 0;
    for (let i = 0; i < strLength; i++) {
      let code = str.charCodeAt(i);
      trueCount += code < 256 ? 1 : 0;
    }
    return (trueCount / strLength) > 0.9;//判断字符串是否全是英文的系数0.9
  }

  const enQuotation = document.getElementsByClassName('tn-on-child-editing')
  if (enQuotation && enQuotation.length > 0) {
    const lastOne = enQuotation[enQuotation.length - 1]
    const allP = lastOne.getElementsByTagName('div')[0]
    // const allP = lastOne.getElementsByTagName('p')

    if (allP && allP.childNodes.length > 0) {
      // 判断字符串是否全是英文
      for (let i = 0; i < allP.childNodes.length; i++) {
        let p = allP.childNodes[i]
        if (p.innerText && p.innerText !== '\n' && p.innerText !== ' ') {
          let isEnglish = isAllEnglish(p.innerText)
          // let isChinese = isAllChinese(p.innerText)
          console.log("isEnglish:", isEnglish);
          const firstP = allP.childNodes[0]
          const secondeP = allP.childNodes[1]
          let enColorDefault = firstP?.childNodes[0]?.nodeName === 'SPAN' ? firstP.childNodes[0].style.color : '#000000'
          let cnColorDefault = secondeP?.childNodes[0]?.nodeName === 'SPAN' ? secondeP.childNodes[0].style.color : '#000000'

          if (isEnglish) {
            let newP = p.innerText
              .replace(/“/g, '"')
              .replace(/”/g, '"')
              .replace(/‘/g, "'")
              .replace(/’/g, "'")
              .replace(/：/g, ":")
              .replace(/；/g, ";")
              .replace(/，/g, ",")
            let newNode = document.createElement('p')
            let newSpan = document.createElement('span')
            newNode.style.textAlign = 'left'
            newSpan.style.color = enColorDefault
            newSpan.textContent = newP
            newNode.appendChild(newSpan)
            allP.replaceChild(newNode, p)
          }
          else {
            let newP1 = p.innerText
              .replace(/"/g, '“')
              .replace(/"/g, '”')
              .replace(/'/g, '‘')
              .replace(/'/g, '’')
              .replace(/:/g, '：')
              .replace(/;/g, '；')
              .replace(/,/g, '，')
            let newNode1 = document.createElement('p')
            let newSpan1 = document.createElement('span')
            newNode1.style.textAlign = 'left'
            newSpan1.style.color = cnColorDefault
            newSpan1.textContent = newP1
            newNode1.appendChild(newSpan1)
            allP.replaceChild(newNode1, p)
          }
        }
      }
    }
  }
}
</script>
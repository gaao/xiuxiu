<template>
  <div>
    <button @click="changeCharacter" title="(“”‘’：，；;)斜体">替换中英文符号\斜体</button>
    <span v-if="hasEM">所有斜体：{{allKeywords}}</span>
  </div>
</template>
<script setup lang="ts">
import { reactive, ref, onMounted } from "vue"
import type { App } from "vue"
import type { PlasmoCSConfig } from "plasmo"
// import popup from "~popup.vue";
import popup from "~popup.vue";

const config: PlasmoCSConfig = {
  matches: ["*://xiumi.us/studio/*"],
}
const hasEM = ref(false)
const allKeywords = ref([])
const findEM = (element) => {
  let emKeywords = ""
  let emAll = element.querySelectorAll("em")
  if (emAll.length > 0) {
    let keywords = []
    hasEM.value = true
    for (let i = 0; i < emAll.length; i++) {
      keywords.push(emAll[i].textContent)
    }
    // 去重
    keywords = Array.from(new Set(keywords))
    allKeywords.value.push(...keywords)
    allKeywords.value= Array.from(new Set(allKeywords.value))
    emKeywords = keywords.join(",")
  }
  return emKeywords
}

function exchangeKeywords(emKeywords: string, newP: string) {
  const keywordsArr = emKeywords.split(",");
  let includeskeywordsNewP = newP;
  for (let j = 0; j < keywordsArr.length; j++) {
    if (newP.includes(keywordsArr[j])) {
      const arr = keywordsArr[j];
      const arrRegex = new RegExp(arr, "g");
      includeskeywordsNewP = includeskeywordsNewP.replace(arrRegex, `<em>${arr}</em>`);
    }
  }
  return includeskeywordsNewP;
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
        if (p.textContent && p.textContent !== '\n' && p.textContent !== ' ') {
          let isEnglish = isAllEnglish(p.textContent)
          // let isChinese = isAllChinese(p.textContent)
          console.log("isEnglish:", isEnglish);
          const firstP = allP.childNodes[0]
          const secondeP = allP.childNodes[1]
          let enColorDefault = firstP?.childNodes[0]?.nodeName === 'SPAN' ? firstP.childNodes[0].style.color : '#000000'
          let cnColorDefault = secondeP?.childNodes[0]?.nodeName === 'SPAN' ? secondeP.childNodes[0].style.color : '#000000'

          if (isEnglish) {
            let newP = p.textContent
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
            // 斜体处理
            let keywords = findEM(p)
            if (hasEM.value) {
              newP = exchangeKeywords(keywords,newP)
            }
            newSpan.innerHTML = newP
            newNode.appendChild(newSpan)
            allP.replaceChild(newNode, p)
          }
          else {
            let newP1 = p.textContent
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
            // 斜体处理
            let keywords = findEM(p)
            if (hasEM.value) {
              newP1 = exchangeKeywords(keywords,newP1)
            }
            newSpan1.innerHTML = newP1
            newNode1.appendChild(newSpan1)
            allP.replaceChild(newNode1, p)
          }
        }
      }
    }
  }
}
</script>
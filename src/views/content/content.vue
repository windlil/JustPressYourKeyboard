<template>
  <div class="my-content">
    <div class="start" v-if="!isStart">PRESS THE SPACE TO ENTER GAME!</div>
    <div class="blocks" v-else>
      <div
        class="block"
        :class="{ circle: isCircle }"
        v-for="item in blocks"
        :style="{ top: item.top, left: item.left, opacity: item.opacity }"
      >
        {{ item.letter }}
      </div>
    </div>
    <div class="time-table">
      <div class="row">
        <span>success:</span>
        <span>{{ success }}</span>
      </div>
      <div class="row">
        <span>wrong:</span>
        <span>{{ wrong }}</span>
      </div>
      <div class="row">
        <span>spent:</span>
        <span>{{ spentTime }}</span>
      </div>
      <div class="restart" @click="restart" v-if="isStart">restart</div>
    </div>
    <div class="setting" v-if="!isStart">
      <div class="setting-row">
        <div class="setting-title" style="color: rgb(254, 165, 179); font-size: 40px;">SETTING</div>
      </div>
      <div class="setting-row">
        <span class="setting-title">block shape:</span>
        <div class="radio">
          <span
            :class="{ actived: currentIndex1 === index }"
            v-for="(item, index) in settingData.shapes"
            @click="changeShape(index, item)"
            >{{ item }}</span
          >
        </div>
      </div>
      <div class="setting-row">
        <span class="setting-title">block quality:</span>
        <div class="radio">
          <span
            :class="{ actived: currentIndex2 === index }"
            v-for="(item, index) in settingData.quality"
            @click="changeQuality(index, item)"
            >{{ item }}</span
          >
        </div>
      </div>
    </div>
    <div class="audio-list" style="display: none"></div>
  </div>
</template>

<script setup>
import { reactive, ref } from "vue"
import mp3Url from "@/static/audio/mouse.mp3"

let settingData = {
  shapes: ["square", "circle"],
  quality: [10, 20, 30, 40, 50],
}

const currentIndex1 = ref(0)
const currentIndex2 = ref(0)

const isStart = ref(false)

function restart() {
  location.reload()
}

const randomBlocks = []
let letterList = "abcdefghijklnmopqrstuvwxyz0123456789"
letterList = letterList.split("")

function getRandomBlocks(blocks, num) {
  for (let i = 0; i < num; i++) {
    const top = (Math.random() * (500 - 10) + 10).toFixed(0) + "px"
    const left = (Math.random() * (1100 - 10) + 10).toFixed(0) + "px"
    const randomIndex = Math.floor(Math.random() * (letterList.length - 1))
    const letter = letterList[randomIndex]
    blocks[i] = {
      top,
      left,
      letter: letter[0],
      opacity: 1,
    }
  }
}

let num = 0

const isCircle = ref(false)
function changeShape(index = 0, item = "square") {
  currentIndex1.value = index
  if (item === "circle") {
    isCircle.value = true
  }
}

function changeQuality(index = 0, item = 10) {
  currentIndex2.value = index
  num = item
  getRandomBlocks(randomBlocks, num)
}

changeShape()
changeQuality()

const blocks = reactive(randomBlocks)
let blocksDelete = ref(blocks.length)

const spentTime = ref(0)
const wrong = ref(0)
const success = ref(0)
let time = 1

function startGame() {
  isStart.value = !isStart.value
  spentTime.value = 0 + "s"
  const startInterval = setInterval(() => {
    if (blocksDelete.value === 0) {
      clearInterval(startInterval)
      return
    }
    spentTime.value = time + "s"
    time++
  }, 1000)

  const audioList = document.querySelector(".audio-list")
  document.addEventListener("keyup", e => {
    const key = e.key
    const lastBlocksDelete = blocksDelete.value
    for (let i = 0; i < blocks.length; i++) {
      const item = blocks[i]
      if (item.letter == key && item.opacity !== 0) {
        if (audioList.children.length > 10) {
          audioList.removeChild(document.querySelector("audio"))
        }
        const audio = document.createElement("audio")
        audio.src = mp3Url
        audioList.appendChild(audio)
        audio.play()
        item.opacity = 0
        blocksDelete.value--
        success.value ++
      }
    }
    if (blocksDelete.value == lastBlocksDelete) {
      wrong.value++
    }
  })
}

function spaceToStart(e) {
  if (e.key === ' ') {
    startGame()
    document.removeEventListener('keyup', spaceToStart)
  }
}

document.addEventListener('keyup', spaceToStart)
</script>

<style lang="less" scoped>
.my-content {
  width: 100vw;
  display: flex;
  justify-content: center;
  margin-top: 40px;

  .start {
    user-select: none;
    font-size: 40px;
    color: #fff;
    font-weight: bold;
    text-shadow: 0 0 15px;
    text-shadow: 0 0 30px;
  }

  .blocks {
    width: 1200px;
    height: 600px;
    position: relative;
    user-select: none;
    .block {
      width: 50px;
      height: 50px;
      position: absolute;
      text-align: center;
      line-height: 50px;
      background-color: #000000;
      color: rgb(255, 0, 255);

      box-shadow: 0 0 36px #000000;
      font-weight: bold;
      font-size: 30px;
      transition: opacity 0.3s;
    }
  }

  .time-table {
    position: absolute;
    right: 20px;
    width: 200px;
    height: 320px;
    background-color: #222222a3;
    border-radius: 20px;
    color: #9e5cb6;
    font-weight: bold;
    padding: 5px;
    .row {
      display: flex;
      span {
        flex: 1;
        margin: 10px;
        text-align: center;
      }
    }
    .restart {
      width: 60%;
      height: 30px;
      line-height: 30px;
      margin: 30px auto;
      background-color: rgb(255, 216, 222);
      text-align: center;
      cursor: pointer;

      &:hover {
        color: #fff;
        background-color: #7f42a0;
      }
    }
  }

  .setting {
    position: absolute;
    top: 260px;
    color: #fff;
    font-size: 22px;
    user-select: none;

    .setting-row {
      margin: 20px;
      display: flex;
      align-items: baseline;

      .setting-title {
        color: #ff8aff;
        font-weight: bold;
        font-size: 26px;
        margin-right: 20px;
      }

      .radio {
        display: flex;
        margin-top: 10px;

        span {
          margin-right: 20px;
          &:hover {
            cursor: pointer;
            text-shadow: 0 0 20px #fff;
          }
        }
      }
    }
  }
}
.actived {
  color: #7a3fee;
  font-weight: bold;
  cursor: default !important;
}

.circle {
  border-radius: 50% !important;
}
</style>

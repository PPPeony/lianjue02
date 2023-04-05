<template>
  <main class="container">
    <div class="content" :class="[state.mode, state.type]">
      <div v-for="item in srcList" :key="item.index" class="box">
        <div class="real-img">
          <span class="order">{{ item.index }}</span>

          <img :src="item.real" alt="" class="img" />
        </div>
        <div class="contral-img">
          <img :src="item.contral" alt="" class="img" />
        </div>
      </div>
    </div>
    <aside class="aside">
      <div class="panel">
        <button
          v-for="item in modeList"
          :key="item.id"
          class="mode-btn"
          :class="state.mode === item.id ? 'orange' : ''"
          @click="
            () => {
              handleModeChange(item.id)
              if (item.id === 'uniform') {
                handleTypeChange('top')
              }
            }
          "
        >
          {{ item.name }}
        </button>
        <div v-if="state.mode === 'uniform'">
          <div class="type">
            <span
              v-for="item in typeList"
              :key="item.id"
              class="item"
              :class="[item.id, state.type === item.id ? 'orange' : '']"
              @click="
                () => {
                  handleTypeChange(item.id)
                }
              "
              >{{ item.name }}</span
            >
          </div>
          <div class="hidden">
            <span>需要隐藏的数据序号</span>
            <input type="text" v-model="state.input" @input="handleInputChange" />
          </div>
        </div>
      </div>
    </aside>
  </main>
</template>

<script setup>
import { reactive, computed } from 'vue'
const modeList = [
  {
    id: 'center',
    name: '中心模式'
  },
  {
    id: 'edge',
    name: '边缘模式'
  },
  {
    id: 'uniform',
    name: '统一模式'
  }
]

const typeList = [
  {
    id: 'top',
    name: '上'
  },
  {
    id: 'bottom',
    name: '下'
  },
  {
    id: 'left',
    name: '左'
  },
  {
    id: 'right',
    name: '右'
  }
]

const imageList = [
  {
    index: 1,
    real: '/src/assets/static/image/1real.png',
    contral: '/src/assets/static/image/1contral.png'
  },
  {
    index: 2,
    real: '/src/assets/static/image/2real.png',
    contral: '/src/assets/static/image/2contral.png'
  },
  {
    index: 3,
    real: '/src/assets/static/image/3real.png',
    contral: '/src/assets/static/image/3contral.png'
  },
  {
    index: 4,
    real: '/src/assets/static/image/4real.png',
    contral: '/src/assets/static/image/4contral.png'
  },
  {
    index: 5,
    real: '/src/assets/static/image/5real.png',
    contral: '/src/assets/static/image/5contral.png'
  },
  {
    index: 6,
    real: '/src/assets/static/image/6real.png',
    contral: '/src/assets/static/image/6contral.png'
  },
  {
    index: 7,
    real: '/src/assets/static/image/7real.png',
    contral: '/src/assets/static/image/7contral.png'
  },
  {
    index: 8,
    real: '/src/assets/static/image/8real.png',
    contral: '/src/assets/static/image/8contral.png'
  }
]

const state = reactive({
  mode: 'center',
  type: 'top',
  input: ''
})

const srcList = computed(() => {
  let hiddenList = state.input.trim().split(',')
  if (!hiddenList.length) {
    return imageList
  } else {
    let targetIndex = 0
    let newList = imageList.filter((item) => {
      if (item.index.toString() !== hiddenList[targetIndex]) {
        return true
      } else {
        targetIndex++
        return false
      }
    })
    return newList
  }
})

const handleModeChange = function (mode) {
  state.mode = mode
}
const handleTypeChange = function (type) {
  state.type = type
}
</script>

<style scoped>
.orange {
  background-color: orange;
}
.container {
  display: flex;
  padding: 20px;
  justify-content: center;
  align-items: center;
  border: 2px solid #333;
}
.content {
  width: 600px;
  height: 550px;
  padding: 20px;
  display: flex;
  flex-wrap: wrap;
  justify-content: start;
  align-content: start;
  gap: 20px;
}
.aside {
  width: 200px;
  flex-shrink: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.panel {
  width: 100%;
  height: 400px;
  display: flex;
  flex-direction: column;
  padding: 20px;
  border: 2px solid #333;
}
.box {
  position: relative;
  width: 124px;
  height: 244px;
  display: flex;
  flex-direction: column;
  border: 2px solid #333;
}

.img {
  vertical-align: middle;
  width: 100px;
  height: 100px;
  margin: 10px;
}
.real-img {
  position: relative;
}
.order {
  position: absolute;
  left: 15px;
  bottom: 10px;
  font-weight: 700;
  color: #fff;
  z-index: 999;
}

.mode-btn {
  margin-bottom: 30px;
}

.center .box .contral-img {
  order: 0;
}
.center .box .real-img {
  order: 1;
}

.center .box:nth-child(n + 5) .real-img {
  order: -1;
}

.edge .box .real-img {
  order: 0;
}
.edge .box .contral-img {
  order: 1;
}
.edge .box:nth-child(n + 5) .contral-img {
  order: -1;
}

.uniform.top .box .real-img {
  order: 0;
}
.uniform.top .box .contral-img {
  order: 1;
}

.uniform.bottom .box .real-img {
  order: 1;
}
.uniform.bottom .box .contral-img {
  order: 0;
}

.uniform.left .box,
.uniform.right .box {
  flex-direction: row;
  width: 124px;
  height: 64px;
}
.uniform.left .img,
.uniform.right .img {
  width: 40px;
  height: 40px;
}
.uniform.left .box .real-img {
  order: 0;
}
.uniform.left .box .contral-img {
  order: 1;
}
.uniform.right .box .real-img {
  order: 1;
}
.uniform.right .box .contral-img {
  order: 0;
}

.hidden {
  display: flex;
  flex-direction: column;
}

.type {
  width: 100%;
  display: grid;
  grid-template-columns: auto auto;
  gap: 10px;
}
.type .item {
  display: block;
  text-align: center;
  border: 2px solid #333;
}
</style>

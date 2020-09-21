<template>
  <div class="body">
    <div class="main">
      <h2 class="title">
        <svg
          t="1592397959998"
          class="icon"
          viewBox="0 0 1024 1024"
          version="1.1"
          xmlns="http://www.w3.org/2000/svg"
          p-id="2557"
          width="128"
          height="128"
        >
          <path
            d="M520.7 99c-148.5 0-268.9 120.4-268.9 268.9S519 890.4 519 890.4s270.5-374 270.5-522.5S669.2 99 520.7 99z m0 342.8c-48.2 0-87.3-39.1-87.3-87.3s39.1-87.3 87.3-87.3 87.3 39.1 87.3 87.3-39.1 87.3-87.3 87.3z"
            fill="#F10215"
            p-id="2558"
          />疫情累计地图
        </svg>
      </h2>
      <Map></Map>
      <h2>
        统计
        <span class="updateTime">更新时间：{{updateTime}}</span>
      </h2>
      <div class="statistics">
        <div>
          <div class="number">{{data.confirm}}</div>
          <div class="desc">确诊病例</div>
        </div>
        <div>
          <div class="number">{{data.suspect}}</div>
          <div class="desc">疑似病例</div>
        </div>
        <div>
          <div class="number">{{data.heal}}</div>
          <div class="desc">治愈人数</div>
        </div>
        <div>
          <div class="number">{{data.dead}}</div>
          <div class="desc">死亡人数</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'
import originData from '../../static/originData.json'
import Map from './Map'
export default {
  components: {
    Map
  },
  setup () {
    const updateTime = ref('')
    const data = ref({})
    onMounted(() => {
      updateTime.value = originData.lastUpdateTime
      data.value = originData.chinaTotal
    })
    return {
      updateTime,
      data
    }
  }
}
</script>

<style lang="scss" scoped>
.main {
  width: 80%;
  margin: 10px auto;
}
.main svg {
  width: 33px;
  height: 33px;
}
.main .title {
  text-decoration: #22aeb4;
}
.main .title::before {
  content: "";
  width: 20px;
  height: 100%;
  background: #22aeb4;
}
.main .title .updateTime {
  font-size: 14px;
  font-weight: 500;
  float: right;
}
.main .statistics {
  display: flex;
  justify-content: space-between;
  text-align: center;
  background-color: #f5f6f7;
  padding: 20px 40px;
  border-radius: 10px;
}
.main .number {
  color: #22aeb4;
  font-size: 20px;
  font-weight: 900;
}
</style>

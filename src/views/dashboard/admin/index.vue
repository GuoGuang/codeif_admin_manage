<template>
  <div class="dashboard-editor-container">
    <github-corner class="github-corner" />

    <panel-group />

    <el-row id="stacked" :gutter="32">
      <el-col :xs="24" :sm="24" :lg="24">
        <!-- <bar-chart/> -->
        <!-- 折线图 -->
        <!-- <rough-line
          data="https://raw.githubusercontent.com/jwilber/random_data/master/profits.csv"
          y1="revenue"
          y2="cost"
          y3="profit"
          title="Visits in the past seven days"
          width="1400"
          height="500"
          radius="18"
          font="0"
        /> -->

        <!-- 堆积面积图 -->
        <rough-stacked-bar
          v-if="width"
          :data="weekAccess"
          :colors="['blue', '#f996ae', 'skyblue', '#9ff4df']"
          labels="day"
          :width="width"
          title="Visits in the past fifteen days"
          height="500"
          roughness="2"
          fill-weight="0.35"
          stroke-width="0.5"
          fill-style="cross-hatch"
          stroke="black"
        />
      </el-col>
    </el-row>

    <el-row :gutter="32">
      <el-col :xs="16" :sm="16" :lg="12">
        <!-- <pie-chart /> -->
        <!-- 饼图 -->
        <rough-pie
          :data="{
            labels: ['Baidu', 'Google', 'Biying', 'Sougou','360'],
            values: [1006, 558, 258,153,725]
          }"
          :colors="['red', 'orange', 'blue', 'skyblue','green']"
          title="System access statistics"
          width="700"
          radius="18"
          font="0"
          roughness="6"
        />
      </el-col>
      <el-col :xs="8" :sm="8" :lg="12">
        <!-- 横线图 -->
        <rough-bar-h
          :margin="{ top: 50, bottom: 100, left: 160, right: 0 }"
          :data="{
            labels: [
              '1992 Ford Aerostar Van',
              '2013 Kia Rio',
              '1980 Honda CB 125s',
              '1992 Toyota Tercel'
            ],
            values: [8, 4, 6, 2]
          }"
          width="600"
          height="500"
          title="Vehicles I've Had"
          title-font-size="1.5rem"
          legend="false"
          x-label="Time Owned (Years)"
          stroke-width="2"
          fill-style="zigzag-line"
          highlight="gold"
        />
      </el-col>
    </el-row>
  </div>
</template>

<script>
import GithubCorner from '@/components/GithubCorner'
import PanelGroup from './components/PanelGroup'

import { RoughStackedBar, RoughPie, RoughBarH } from 'vue-roughviz'

export default {
  name: 'DashboardAdmin',
  components: {
    GithubCorner,
    PanelGroup,
    RoughStackedBar, RoughPie, RoughBarH
  },
  data() {
    return {
      weekAccess: [],
      width: 0
    }
  },
  created() {
    this.weekAccess = this.getWeek()
  },
  mounted() {
    this.width = document.getElementById('stacked').clientWidth - 50
  },
  methods: {
    getWeek() {
      var myDate = new Date()
      myDate.setDate(myDate.getDate() - 7)
      var dateArray = []
      var flag = 1
      for (var i = 0; i < 15; i++) {
        dateArray.push({
          day: (myDate.getMonth() + 1) + '-' + myDate.getDate(),
          A: Math.floor(Math.random() * 109),
          B: Math.floor(Math.random() * 300),
          C: Math.floor(Math.random() * 50)
        })
        myDate.setDate(myDate.getDate() + flag)
      }
      return dateArray
    }

  }
}
</script>

<style lang="scss" scoped>
.dashboard-editor-container {
  background-color: rgb(240, 242, 245);
  position: relative;
  .github-corner {
    z-index: 1;
    position: absolute;
    top: 0px;
    border: 0;
    right: 0;
  }
  .chart-wrapper {
    background: #fff;
    padding: 16px 16px 0;
    margin-bottom: 32px;
  }
  .project-list {
    padding-right: 25px;
    .project-item {
      display: flex;
      border: 1px solid #f1f1f1;
      .project-avatar {
        margin: 0.5em;
        padding-top: 0.5em;
      }
      .project-name {
        margin-top: 0.5em;
      }
    }
  }
}
</style>

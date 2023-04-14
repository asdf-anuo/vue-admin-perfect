<template>
  <div class="contentRight">
    <div class="right-item1" style="background-image: url('static/healthBigData/rightTop.png')">
      <Title-Comp title="物资管理概况" />
      <div class="topStatic">
        <div class="publicItem">
          <img src="static/healthBigData/iconOrg.png" />
          <span class="text-top publicTxtColor">床位使用率</span>
          <span class="numTxtShadow1">{{ bedUsageStatistic.usageRate || 0 }}</span>
          <span class="text-top publicTxtColor">%</span>
        </div>
        <div class="publicItem">
          <img src="static/healthBigData/iconRateS.png" />
          <span class="text-top publicTxtColor">较去年</span>
          <span class="numTxtShadow1">{{ bedUsageStatistic.bedNumRate || 0 }}</span>
          <span class="text-top publicTxtColor">%</span>
          <img v-if="bedUsageStatistic.bedNumRate > 0" src="static/healthBigData/iconRateUp.png" />
          <img
            v-if="bedUsageStatistic.bedNumRate < 0"
            src="static/healthBigData/iconRateDown.png"
          />
        </div>
      </div>
      <div class="bottomStatic">
        <div class="publicItem">
          <div class="text-bottom publicTxtColor">家庭居民签约数</div>
          <div class="numTxtShadow2"
            >{{ jjm || 0 }} <span class="text-bottom publicTxtColor">个</span></div
          >
        </div>
        <div class="publicItem">
          <div class="text-bottom publicTxtColor">家医家庭签约数</div>
          <div class="numTxtShadow2"
            >{{ jys || 0 }} <span class="text-bottom publicTxtColor">个</span></div
          >
        </div>
        <div class="publicItem">
          <div class="text-bottom publicTxtColor">家庭团队数</div>
          <div class="numTxtShadow2"
            >{{ team || 0 }} <span class="text-bottom publicTxtColor">个</span></div
          >
        </div>
      </div>
    </div>
    <div
      class="right-item2 cardBgColor"
      style="background-image: url('static/healthBigData/rightBottom.png')"
    >
      <Title-Comp title="家庭医生签约" />
      <div v-if="false" class="detail" @click="redrictClick">
        <a href="#">MORE</a>
      </div>
      <div id="dataQuality"></div>
    </div>
  </div>
</template>

<script>
  import TitleComp from './TitleComp'
  import * as echarts from 'echarts'
  export default {
    name: 'ContentRight',
    components: { TitleComp },
    data() {
      return {
        bedUsageStatistic: {
          usageRate: 0,
          bedNumRate: 0,
        },
        jjm: '',
        jys: '',
        team: '',
      }
    },
    props: ['pubicData'],
    watch: {
      pubicData(newValue, oldVaule) {
        if (newValue) {
          this.bedUsageStatistic = this.pubicData['bedUsageStatistic']
          let dt = this.pubicData['familyDoctorSign']
          let seriesData = [
            dt['oldPeopleSignNum'],
            dt['diabetesSignNum'],
            dt['hypertensionSignNum'],
            dt['fiveGuaranteeSignNum'],
            dt['subsistenceSignNum'],
            dt['poorPeopleSignNum'],
          ]
          this.dataQuality(seriesData)
          this.jjm = dt['residentSignNum'] || 0
          this.jys = dt['familySignNum'] || 0
          this.team = dt['familyDoctorNum'] || 0
        }
      },
    },
    methods: {
      redrictClick() {
        this.$router.push('/supervision/medicalservice')
      },
      drawChart(id, option) {
        let myChart = echarts.init(document.getElementById(id))
        myChart.setOption(option)
      },
      dataQuality(list) {
        let xLabelAray = ['65岁以\n上老人', '糖尿病', '高血压', '五保户', '低保户', '贫困人口']
        let option = {
          color: ['#e2c166', '#e8784f', '#f6f7ea'],
          tooltip: {
            trigger: 'axis',
            axisPointer: {
              type: 'cross',
              crossStyle: {
                color: '#999',
              },
              label: {
                formatter: function (params) {
                  let value = params.value
                  if (typeof value === 'number') {
                    return value.toFixed(2)
                  }
                },
              },
            },
          },
          grid: {
            left: '3%',
            right: '4%',
            bottom: '3%',
            containLabel: true,
          },
          xAxis: [
            {
              type: 'category',
              data: xLabelAray,
              axisLabel: {
                margin: 20,
                color: '#a1a6bd',
              },
              axisPointer: {
                type: 'shadow',
              },
              axisLine: {
                lineStyle: {
                  color: '#505a87',
                },
              },
            },
          ],
          yAxis: [
            {
              type: 'value',
              name: '人数',
              nameTextStyle: {
                color: '#a1a6bd',
              },
              axisLine: {
                lineStyle: {
                  color: '#999',
                },
              },
              splitLine: {
                lineStyle: {
                  color: 'transprent',
                },
              },
            },
          ],
          series: [
            {
              name: '人数',
              type: 'bar',
              barWidth: '30%',
              label: {
                show: true,
                position: 'top',
                offset: [0, 0],
                color: '#7fb6c9',
                formatter: function (params) {
                  return params.value
                },
              },
              itemStyle: {
                normal: {
                  color: {
                    type: 'linear',
                    x: 0,
                    y: 0,
                    x2: 0,
                    y2: 1,
                    colorStops: [
                      {
                        offset: 1,
                        color: 'rgb(200,190,190)',
                      },
                      {
                        offset: 0,
                        color: '#e8784f',
                      },
                    ],
                  },
                },
              },
              data: list,
            },
          ],
        }
        this.drawChart('dataQuality', option)
      },
    },
  }
</script>

<style lang="less" scoped>
  .contentRight {
    display: flex;
    flex-direction: column;
  }
  .right-item1 {
    height: 200px;
    background-size: 100% 100%;
    position: relative;
    padding: 10px;
    .topStatic {
      display: flex;
      flex-direction: row;
      justify-content: space-between;
      margin-top: 30px;
      margin-bottom: 40px;
      padding: 10px;
      background-color: rgba(255, 255, 255, 0.05);
      .publicItem {
        display: inline-block;
        .text-top {
          font-size: 12px;
        }
      }
    }
    .bottomStatic {
      display: flex;
      flex-direction: row;
      justify-content: space-between;
      margin-top: 20px;

      .publicItem {
        display: flex;
        flex-direction: column;
        .text-bottom {
          font-size: 12px;
        }
      }
    }
  }
  .right-item2 {
    flex: 1;
    margin-top: 30px;
    background-size: 100% 100%;
    position: relative;
    .detail {
      position: absolute;
      top: 8px;
      right: 6px;
      font-size: 12px;
      z-index: 1000;
    }
    #dataQuality {
      width: 100%;
      height: 100%;
    }
  }
</style>

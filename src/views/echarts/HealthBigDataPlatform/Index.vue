<template>
  <div class="mcs-main" style="background-image: url('static/healthBigData/bg.jpg')">
    <div class="top-header">
      <div class="leftSubTitle shadowTxt">{{ topLeftTitle }}</div>
      <div class="rightDateBox shadowTxt">
        <span>{{ topRightTime }}</span>
        <span>{{ topRightDate }}</span>
        <span>{{ topRightWeekday }}</span>
      </div>
    </div>
    <div class="content-main">
      <div class="content-left">
        <div
          class="left-item1 cardBgColor"
          style="background-image: url('static/healthBigData/leftTop.png')"
        >
          <Title-Comp title="机构接入概况" />
          <div class="topStatic">
            <div class="publicItem">
              <img src="static/healthBigData/iconOrg.png" />
              <span class="text-top publicTxtColor">机构总人数</span>
              <span class="counts-top numTxtShadow1">{{
                orgStatistic.orgTotalPersonNum || 0
              }}</span>
              <span class="text-top publicTxtColor">人</span>
            </div>
            <div class="publicItem">
              <img src="static/healthBigData/iconRateS.png" />
              <span class="text-top publicTxtColor">较去年</span>
              <span class="counts-top numTxtShadow1">{{
                orgStatistic.beforLastYearRate || 0
              }}</span>
              <span class="text-top publicTxtColor">%</span>
              <img
                v-if="orgStatistic.beforLastYearRate > 0"
                src="static/healthBigData/iconRateUp.png"
              />
              <img
                v-if="orgStatistic.beforLastYearRate < 0"
                src="static/healthBigData/iconRateDown.png"
              />
            </div>
          </div>
          <div class="bottomStatic">
            <div class="publicItem">
              <div class="text-bottom publicTxtColor">三级机构</div>
              <div class="counts-bottom">
                <span class="numTxtShadow2">{{ orgStatistic.level3OrgNum || 0 }}</span>
                <span class="text-bottom publicTxtColor">家</span>
              </div>
            </div>
            <div class="publicItem">
              <div class="text-bottom publicTxtColor">二级机构</div>
              <div class="counts-bottom">
                <span class="numTxtShadow2">{{ orgStatistic.level2OrgNum || 0 }}</span>
                <span class="text-bottom publicTxtColor">家</span>
              </div>
            </div>
            <div class="publicItem">
              <div class="text-bottom publicTxtColor">基层医疗机构</div>
              <div class="counts-bottom">
                <span class="numTxtShadow2">{{ orgStatistic.grassRootOrgNum || 0 }}</span>
                <span class="text-bottom publicTxtColor">家</span>
              </div>
            </div>
            <div class="publicItem">
              <div class="text-bottom publicTxtColor">村卫生室</div>
              <div class="counts-bottom">
                <span class="numTxtShadow2">{{ orgStatistic.villageClinicNum || 0 }}</span>
                <span class="text-bottom publicTxtColor">家</span>
              </div>
            </div>
          </div>
        </div>
        <div
          class="left-item2 cardBgColor"
          style="background-image: url('static/healthBigData/leftBottom.png')"
        >
          <Title-Comp title="医保基金使用概况" />
          <div id="healthRes"></div>
        </div>
      </div>
      <content-center class="content-certent" :pubicData="medicaldata" />
      <content-right class="content-right" :pubicData="medicaldata" />
    </div>
  </div>
</template>
<script>
  import ContentCenter from './ContentCenter'
  import ContentRight from './ContentRight'
  import TitleComp from './TitleComp'
  import * as echarts from 'echarts'
  export default {
    name: 'MscHomePage',
    components: { ContentCenter, ContentRight, TitleComp },
    data() {
      return {
        mockData: {
          orgStatistic: {
            level3OrgNum: 1,
            level2OrgNum: 3,
            grassRootOrgNum: 9,
            villageClinicNum: 88,
            orgTotalPersonNum: 518196,
            beforLastYearRate: 1.5,
            totalPersonNum: 518196,
          },
          hospitalResource: null,
          medInsUsage: [
            {
              orgType: '1',
              totalFee: 48268772.71,
              ownFee: 46595616.89,
              medInsFee: 1673155.82,
            },
            {
              orgType: '2',
              totalFee: 12214837.3,
              ownFee: 3952003.03,
              medInsFee: 8262834.27,
            },
            {
              orgType: '3',
              totalFee: 11337568.84,
              ownFee: 5654843.2,
              medInsFee: 5682725.64,
            },
            {
              orgType: '4',
              totalFee: 23623710.33,
              ownFee: 11716688.88,
              medInsFee: 11907021.45,
            },
          ],
          recordResource: {
            ehrNum: 304741,
            emrNum: 97284,
          },
          collaborationStatistic: {
            recordVisitNum: 301,
            remoteDiagnoseNum: null,
            remoteImageNum: 27449,
            remoteCardiogramNum: 5062,
          },
          treatmentStatistic: {
            outpatientIncome: 42123824.2,
            outpatientCount: 288444,
            inHosIncome: 35878919.56,
            inHosCount: 7780,
          },
          publicHealthStatistic: {
            diabetesRecordNum: 7309,
            hypertensionRecordNum: 30189,
            oldPeopleRecordNum: 44133,
            psychosisRecordNum: 1598,
            poorRecordNum: 32134,
            tuberculosisRecordNum: 1174,
            childRecordNum: null,
            poorHelpRecordNum: null,
            lateBloodDiseaseRecordNum: null,
            disabledRecordNum: null,
            familyPlanRecordNum: null,
          },
          familyDoctorSign: {
            residentSignNum: 67520,
            familySignNum: 67520,
            familyDoctorNum: 113,
            poorPeopleSignNum: 13556,
            subsistenceSignNum: 63,
            fiveGuaranteeSignNum: 13556,
            hypertensionSignNum: 20,
            diabetesSignNum: 3884,
            oldPeopleSignNum: 16385,
          },
          bedUsageStatistic: {
            totalBedNum: 123,
            bedOccupyNum: 12,
            usageRate: 112,
            bedNumRate: -2.1,
          },
          pneumoniaStatistic: null,
          epidemicPrevention: {
            residentNum: 518196,
            nucleinResultNum: 9360542,
            vaccinationNum: 912214,
          },
        },
        orgStatistic: {
          orgTotalPersonNum: 0,
          beforLastYearRate: 0,
          level3OrgNum: 0,
          level2OrgNum: 0,
          grassRootOrgNum: 0,
          villageClinicNum: 0,
        },
        topLeftTitle: 'xx市健康大数据中心',
        topRightTime: '',
        topRightDate: '',
        topRightWeekday: '',
        timer: null,
        medicaldata: null,
      }
    },
    methods: {
      //获取 时分秒 年月日 星期几
      getCurrentDate() {
        const nowDate = new Date()
        const date = {
          year: nowDate.getFullYear(),
          month: nowDate.getMonth() + 1,
          date: nowDate.getDate(),
          week: nowDate.getDay() - 1,
          hours: nowDate.getHours(),
          minutes: nowDate.getMinutes(),
          seconds: nowDate.getSeconds(),
        }
        const newmonth = date.month > 9 ? date.month : '0' + date.month
        const newday = date.date > 9 ? date.date : '0' + date.date
        this.topRightDate = `${date.year}/${newmonth}/${newday}`
        const newminutes = date.minutes > 9 ? date.minutes : '0' + date.minutes
        const newseconds = date.seconds > 9 ? date.seconds : '0' + date.seconds
        this.topRightTime = `${date.hours}:${newminutes}:${newseconds}`
        let weekDays = ['星期日', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六']
        this.topRightWeekday = weekDays[date.week]
      },
      drawChart(id, option) {
        let myChart = echarts.init(document.getElementById(id))
        myChart.setOption(option)
      },
      nullToStr(data) {
        for (let i in data) {
          if (data[i] == null) {
            data[i] = 0
          }
        }
        return data
      },
      getXianTaoData() {
        // 智慧医疗大数据首页数据获取
        this.medicaldata = this.mockData
        this.orgStatistic = this.medicaldata['orgStatistic']
        this.orgStatistic.level3OrgNum = this.orgStatistic['level3OrgNum']
          ? this.orgStatistic['level3OrgNum']
          : 0
        this.orgStatistic.level2OrgNum = this.orgStatistic['level2OrgNum']
          ? this.orgStatistic['level2OrgNum']
          : 0
        this.orgStatistic.grassRootOrgNum = this.orgStatistic['grassRootOrgNum']
          ? this.orgStatistic['grassRootOrgNum']
          : 0
        this.orgStatistic.villageClinicNum = this.orgStatistic['villageClinicNum']
          ? this.orgStatistic['villageClinicNum']
          : 0
        this.orgStatistic.orgTotalPersonNum = this.orgStatistic['orgTotalPersonNum']
          ? this.orgStatistic['orgTotalPersonNum']
          : 0
        this.orgStatistic.beforLastYearRate = this.orgStatistic['beforLastYearRate']
          ? this.orgStatistic['beforLastYearRate']
          : 0
        this.healthRes(this.medicaldata['medInsUsage'])
      },
      translateValue(value) {
        let danwei1 = Math.pow(10, 2) // 先除两位，再四舍五入，再保留两位
        let danwei2 = Math.pow(10, 2) // 先除两位，再四舍五入，再保留两位
        value = value / danwei1
        value = Math.round(value)
        value = value / danwei2
        return value
      },
      healthRes(dt) {
        dt = dt || []
        let orgTypeName = {
          type1: '市直医院',
          type2: '民营医院',
          type3: '公立医院',
          type4: '卫生室',
        }
        // eslint-disable-next-line @typescript-eslint/no-this-alias
        let self = this
        let org = [],
          ownFee = [],
          totalFee = [],
          medInsFee = []
        dt.forEach(function (item) {
          let n_ownFee = item['ownFee']
          let n_totalFee = item['totalFee']
          let n_medInsFee = item['medInsFee']
          n_ownFee = self.translateValue(n_ownFee)
          n_totalFee = self.translateValue(n_totalFee)
          n_medInsFee = self.translateValue(n_medInsFee)
          let key = item['orgType']
          org.push(orgTypeName['type' + key])
          ownFee.push(n_ownFee)
          totalFee.push(n_totalFee)
          medInsFee.push(n_medInsFee)
          totalFee.push(n_totalFee)
          medInsFee.push(n_medInsFee)
        })
        let option = {
          title: {},
          grid: {
            top: 80,
            bottom: 50,
            left: '15%',
            right: '8%',
          },
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
          legend: {
            top: 40,
            right: 2,
            textStyle: {
              color: '#a1a6bd',
              fontSize: '12px',
            },
            data: ['医保', '自费'],
          },
          xAxis: [
            {
              type: 'category',
              data: org,
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
              name: '费用（万元）',
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
              name: '医保',
              type: 'bar',
              barWidth: '16%',
              label: {
                show: true,
                position: 'top',
                // rotate: 15,
                offset: [-15, 0],
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
                        color: '#e2c166',
                      },
                    ],
                  },
                },
              },
              // stack: "基金使用情况",
              data: ownFee,
            },
            {
              name: '自费',
              type: 'bar',
              barWidth: '16%',
              label: {
                show: true,
                position: 'top',
                // rotate: 15,
                offset: [15, 0],
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
              // stack: "基金使用情况",
              data: medInsFee,
            },
          ],
        }
        this.drawChart('healthRes', option)
      },
    },
    mounted() {
      // eslint-disable-next-line @typescript-eslint/no-this-alias
      let self = this
      this.timer = setInterval(() => {
        self.getCurrentDate()
      }, 1000)
      this.getXianTaoData()
    },
    beforeUnmount: function () {
      if (this.timer) {
        // 在Vue实例销毁前，清除我们的定时器
        clearInterval(this.timer)
      }
    },
  }
</script>

<style lang="less" scope>
  .top-header {
    height: 8%;
    width: 98%;
    margin: 0 1%;
    background-size: 100% 100%;
    position: relative;
    .leftSubTitle {
      position: absolute;
      top: 18%;
      left: 8px;
      color: #f3f3f3;
      font-size: 20px;
      font-weight: 800;
      font-style: italic;
      letter-spacing: 2px;
    }
    .middleBigTitle {
      position: absolute;
      top: 6%;
      left: 50%;
      translate: -50%;
      color: #fff;
      font-size: 36px;
      font-weight: 800;
      font-style: italic;
      letter-spacing: 6px;
    }
    .rightDateBox {
      position: absolute;
      top: 18%;
      right: 2px;
      color: #f3f3f3;
      font-size: 18px;
      font-weight: 600;
      letter-spacing: 1px;
      span {
        display: inline-block;
        margin: 0 4px;
      }
    }
    .shadowTxt {
      text-shadow: 0 0 8px #535353, 0 0 8px #5a5a5a, 0 0 2px #646464, 0 0 2px #646464;
    }
  }

  .mcs-main {
    padding-right: 20px;
    width: 100%;
    height: 100%;
    background: white;
    min-width: 1500px;
    min-height: 900px;
    background-size: 100% 100%;
    .content-main {
      width: 100%;
      height: 90%;
      margin: 10px;
      display: flex;
      .content-left {
        width: 27%;
        display: flex;
        flex-direction: column;
        .left-item1 {
          background-size: 100% 100%;
          height: 200px;
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
              .counts-top {
                margin-left: 8px;
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
              .counts-bottom {
                color: #fff;
              }
            }
          }
        }
        .left-item2 {
          flex: 1;
          margin-top: 30px;
          background-size: 100% 100%;
          position: relative;
          #healthRes {
            width: 100%;
            height: 100%;
          }
        }
      }
      .content-certent {
        width: 45%;
        margin: 0 1%;
        height: 100%;
        display: flex;
        padding: 0 5px;
        flex-direction: column;
      }
      .content-right {
        width: 27%;
        display: flex;
        flex-direction: column;
      }
    }
  }

  .numTxtShadow1 {
    color: #fff;
    font-size: 20px;
    font-weight: 600;
    text-shadow: 0 0 2px #757575, 0 0 2px #666666, 0 0 2px #a3a3a3, 0 0 2px #a3a3a3;
  }
  .numTxtShadow2 {
    color: #fff;
    font-size: 24px;
    font-weight: 700;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    background-image: -webkit-linear-gradient(
      bottom,
      rgb(80, 69, 240),
      #dfdfdf,
      rgb(255, 255, 255)
    );
  }
  .cardBgColor {
    background-color: #0a0a2e;
    border-radius: 5px;
  }
  .publicTxtColor {
    color: #e4e4e4;
  }
</style>

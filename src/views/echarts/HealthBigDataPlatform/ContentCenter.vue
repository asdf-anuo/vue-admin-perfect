<template>
  <div class="middleWarp">
    <div class="countWrap" style="background-image: url('static/healthBigData/centerTop.png')">
      <div class="count-item">
        <div class="count-title publicTxtColor">居民健康档案数</div>
        <div class="numTxtShadow2" id="healthRecord">{{ healthRecord || 0 }}</div>
      </div>
      <div class="count-item">
        <div class="count-title publicTxtColor">电子病历</div>
        <div class="numTxtShadow2" id="eleMedicalRecord">{{ eleMedicalRecord || 0 }}</div>
      </div>
      <div class="count-item">
        <div class="count-title publicTxtColor">健康档案调阅人次</div>
        <div class="numTxtShadow2" id="readTimes">{{ readTimes || 0 }}</div>
      </div>
      <div class="count-item">
        <div class="count-title publicTxtColor">远程影像</div>
        <div class="numTxtShadow2" id="referralNum">{{ remoteImageNum || 0 }}</div>
      </div>
      <div class="count-item">
        <div class="count-title publicTxtColor">远程心电</div>
        <div class="numTxtShadow2" id="remoteTimes">{{ remoteCardiogramNum || 0 }}</div>
      </div>
    </div>
    <div
      class="medical-middle"
      style="background-image: url('static/healthBigData/centerMiddle.png')"
    >
      <Title-Comp title="医疗服务概况" />
      <div class="medLeftChart" id="medServiceLeft"></div>
      <div class="chartMedTxt">
        <div class="ringLine">
          <div class="dot dotColor1"><span class="txtStyle1">住院收入</span></div>
          <div class="dotNum"
            ><span class="numTxtShadow2">{{ inHosIncome || 0 }}</span
            >万</div
          >
        </div>
        <div class="ringLine">
          <div class="dot dotColor2"><span class="txtStyle2">门诊收入</span></div>
          <div class="dotNum"
            ><span class="numTxtShadow2">{{ outpatientIncome || 0 }}</span
            >万</div
          >
        </div>
      </div>
      <div class="medRightChart" id="medServiceRight"></div>
      <div class="chartMedTxt">
        <div class="ringLine">
          <div class="dot dotColor1"><span class="txtStyle1">住院人次</span></div>
          <div class="dotNum"
            ><span class="numTxtShadow2">{{ inHosCount || 0 }}</span></div
          >
        </div>
        <div class="ringLine">
          <div class="dot dotColor2"><span class="txtStyle2">门诊人次</span></div>
          <div class="dotNum"
            ><span class="numTxtShadow2">{{ outpatientCount || 0 }}</span></div
          >
        </div>
      </div>
    </div>
    <div
      class="medical-bottom cardBgColor"
      style="background-image: url('static/healthBigData/centerBottom.png')"
    >
      <Title-Comp title="公卫服务概况" />
      <div class="publicLeftChart" id="publicLeft"></div>
      <div class="chartPublicTxt">
        <div class="ringLine">
          <div class="dot dotColor1"><span class="txtStyle1">个人建档数</span></div>
          <div class="dotNum"
            ><span class="numTxtShadow2">{{ gerenDoc || 0 }}</span
            >万</div
          >
        </div>
        <div class="ringLine">
          <div class="dot dotColor2"><span class="txtStyle2">未建档数</span></div>
          <div class="dotNum"
            ><span class="numTxtShadow2">{{ nullDoc || 0 }}</span
            >万</div
          >
        </div>
      </div>
      <div class="publicRightList item-scroll">
        <div class="publicItem">
          <div class="files-text publicTxtColor">高血压人群档案数量</div>
          <div class="files-num">{{ gxy || 0 }}</div>
        </div>
        <div class="publicItem">
          <div class="files-text publicTxtColor">糖尿病人群档案数量</div>
          <div class="files-num">{{ tnb || 0 }}</div>
        </div>
        <div class="publicItem">
          <div class="files-text publicTxtColor">老年人群档案数量</div>
          <div class="files-num">{{ oldman || 0 }}</div>
        </div>
        <div class="publicItem">
          <div class="files-text publicTxtColor">严重精神障碍人群档案梳数量</div>
          <div class="files-num">{{ jsb || 0 }}</div>
        </div>
        <div class="publicItem">
          <div class="files-text publicTxtColor">贫困人群档案数量</div>
          <div class="files-num">{{ poor || 0 }}</div>
        </div>
        <div class="publicItem">
          <div class="files-text publicTxtColor">肺结核人群档案数量</div>
          <div class="files-num">{{ fjh || 0 }}</div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
  import TitleComp from './TitleComp'
  import * as echarts from 'echarts'
  export default {
    name: 'ContentCenter',
    components: { TitleComp },
    data() {
      return {
        healthRecord: '',
        eleMedicalRecord: '',
        readTimes: '',
        remoteTimes: '',
        gxy: '',
        tnb: '',
        oldman: '',
        jsb: '',
        poor: '',
        fjh: '',
        outpatientIncome: '',
        inHosIncome: '',
        outpatientCount: '',
        inHosCount: '',
        gerenDoc: 0,
        nullDoc: 0,
        totalPeople: 0,
        remoteImageNum: '',
        remoteCardiogramNum: '',
      }
    },
    props: ['pubicData'],
    watch: {
      pubicData(newValue, oldVaule) {
        if (newValue) {
          let medicalServicedata = this.pubicData['treatmentStatistic']
          this.outpatientIncome = medicalServicedata.outpatientIncome
            ? parseInt(Number(medicalServicedata.outpatientIncome) / 10000)
            : 0
          this.inHosIncome = medicalServicedata.inHosIncome
            ? parseInt(Number(medicalServicedata.inHosIncome) / 10000)
            : 0
          this.outpatientCount = medicalServicedata.outpatientCount
            ? medicalServicedata.outpatientCount
            : 0
          this.inHosCount = medicalServicedata.inHosCount ? medicalServicedata.inHosCount : 0
          this.drawLeftPie()
          this.drawRightPie()
          this.drawPublicLeft()
          let dt = this.pubicData['familyDoctorSign']
          this.jjm = dt['residentSignNum'] ? dt['residentSignNum'] : 0
          this.jys = dt['familySignNum'] ? dt['familySignNum'] : 0
          this.team = dt['familyDoctorNum'] ? dt['familyDoctorNum'] : 0
          let d2 = this.pubicData['publicHealthStatistic']
          this.gxy = d2['hypertensionRecordNum'] ? d2['hypertensionRecordNum'] : 0
          this.tnb = d2['diabetesRecordNum'] ? d2['diabetesRecordNum'] : 0
          this.oldman = d2['oldPeopleRecordNum'] ? d2['oldPeopleRecordNum'] : 0
          this.jsb = d2['psychosisRecordNum'] ? d2['psychosisRecordNum'] : 0
          this.poor = d2['poorRecordNum'] ? d2['poorRecordNum'] : 0
          this.fjh = d2['tuberculosisRecordNum'] ? d2['tuberculosisRecordNum'] : 0
          let tD1 = this.pubicData['recordResource']
          let tD2 = this.pubicData['collaborationStatistic']
          this.healthRecord = tD1['ehrNum'] ? tD1['ehrNum'] : 0
          this.eleMedicalRecord = tD1['emrNum'] ? tD1['emrNum'] : 0
          this.readTimes = tD2['recordVisitNum'] ? tD2['recordVisitNum'] : 0
          this.remoteTimes = tD2['remoteDiagnoseNum'] ? tD2['remoteDiagnoseNum'] : 0
          this.remoteImageNum = tD2['remoteImageNum'] ? tD2['remoteImageNum'] : 0
          this.remoteCardiogramNum = tD2['remoteCardiogramNum'] ? tD2['remoteCardiogramNum'] : 0
        }
      },
    },
    methods: {
      jumpPage(type) {
        this.$mcsbaseUrl = process.env.API_ROOT
        if (type === 1) {
          let _url = 'index.html'
          this.$router.push('/platformapp/healthbrowser')
        } else {
          let _url = 'xt/index.html'
          this.$router.push('/supervision/medicalservice')
        }
      },
      drawEchart(id, option) {
        let myChart = echarts.init(document.getElementById(id))
        myChart.setOption(option)
      },
      drawLeftPie() {
        let optionLeft = {
          title: {
            text: '',
          },
          tooltip: {
            trigger: 'item',
            formatter: '{b}: {c}w ({d}%)',
          },
          series: [
            {
              name: '',
              type: 'pie',
              center: ['50%', '50%'],
              radius: ['50%', '60%'],
              data: [
                {
                  value: this.outpatientIncome,
                  name: '门诊收入',
                  itemStyle: {
                    color: '#BEF192',
                  },
                  label: {
                    normal: {
                      show: false,
                      position: 'center',
                      color: '#BEF192',
                      fontSize: 12,
                      formatter: '{b}\n{c}万',
                    },
                  },
                  emphasis: {
                    label: {
                      show: true,
                      position: 'center',
                      fontSize: 14,
                      fontWeight: 'bold',
                    },
                  },
                },
                {
                  value: this.inHosIncome,
                  name: '住院收入',
                  itemStyle: {
                    color: '#04D9FE',
                  },
                  label: {
                    normal: {
                      show: false,
                      position: 'center',
                      color: '#04D9FE',
                      fontSize: 12,
                      formatter: '{b}\n{c}万',
                    },
                  },
                  emphasis: {
                    label: {
                      show: true,
                      position: 'center',
                      fontSize: 14,
                      fontWeight: 'bold',
                    },
                  },
                },
              ],
            },
          ],
        }
        this.drawEchart('medServiceLeft', optionLeft)
      },
      drawRightPie() {
        let optionRight = {
          tooltip: {
            trigger: 'item',
            formatter: '{b}: {c} ({d}%)',
          },
          series: [
            {
              name: '',
              type: 'pie',
              center: ['50%', '50%'],
              radius: ['50%', '60%'],
              data: [
                {
                  value: this.outpatientCount,
                  name: '门诊人次',
                  itemStyle: {
                    color: '#BEF192',
                  },
                  label: {
                    normal: {
                      show: false,
                      position: 'center',
                      fontSize: 12,
                      formatter: '{b}\n{c}',
                    },
                  },
                  emphasis: {
                    label: {
                      show: true,
                      position: 'center',
                      fontSize: 14,
                      fontWeight: 'bold',
                    },
                  },
                },
                {
                  value: this.inHosCount,
                  name: '住院人次',
                  itemStyle: {
                    color: '#04D9FE',
                  },
                  label: {
                    normal: {
                      show: false,
                      position: 'center',
                      fontSize: 12,
                      color: '#04D9FE',
                      formatter: '{b}\n{c}',
                    },
                  },
                  emphasis: {
                    label: {
                      show: true,
                      position: 'center',
                      fontSize: 14,
                      fontWeight: 'bold',
                    },
                  },
                },
              ],
            },
          ],
        }
        this.drawEchart('medServiceRight', optionRight)
      },
      drawPublicLeft() {
        if (
          this.pubicData['recordResource']['ehrNum'] &&
          this.pubicData['orgStatistic']['totalPersonNum']
        ) {
          this.gerenDoc = (Number(this.pubicData['recordResource']['ehrNum']) / 10000).toFixed(1)
          this.totalPeople = (
            Number(this.pubicData['orgStatistic']['totalPersonNum']) / 10000
          ).toFixed(1)
          this.nullDoc = Number(this.totalPeople - this.gerenDoc).toFixed(1)
        } else {
          this.gerenDoc = 0
          this.nullDoc = 0
        }
        let optionLeft = {
          title: {
            text: '',
          },
          tooltip: {
            trigger: 'item',
            formatter: '{b}: {c}w ({d}%)',
          },
          series: [
            {
              name: '',
              type: 'pie',
              center: ['50%', '50%'],
              radius: ['50%', '60%'],
              avoidLabelOverlap: false,
              data: [
                {
                  value: this.gerenDoc,
                  name: '个人建档数',
                  itemStyle: {
                    color: '#04D9FE',
                  },
                  label: {
                    normal: {
                      show: false,
                      position: 'center',
                      formatter: '个人建档数\n\n{c}万份',
                      textStyle: {
                        color: '#04D9FE',
                        fontSize: 12,
                        fontWeight: 'bold',
                      },
                    },
                  },
                  emphasis: {
                    label: {
                      show: true,
                      position: 'center',
                      fontSize: 14,
                      fontWeight: 'bold',
                    },
                  },
                },
                {
                  value: this.nullDoc,
                  name: '未建档数',
                  itemStyle: {
                    color: '#BEF192',
                  },
                  label: {
                    normal: {
                      show: false,
                      position: 'center',
                      formatter: '未建档数\n\n{c}万份',
                      textStyle: {
                        fontSize: 12,
                        fontWeight: 'bold',
                        color: '#BEF192',
                      },
                    },
                  },
                  emphasis: {
                    label: {
                      show: true,
                      position: 'center',
                      fontSize: 14,
                      fontWeight: 'bold',
                    },
                  },
                },
              ],
            },
          ],
        }
        this.drawEchart('publicLeft', optionLeft)
      },
    },
  }
</script>
<style lang="less" scope>
  .middleWarp {
    display: flex;
    flex-direction: column;
  }
  .countWrap {
    background-size: 100% 100%;
    width: 100%;
    height: 100px;
    height: 100px;
    text-align: center;
    display: flex;
    padding-top: 30px;
    position: relative;
    .count-item {
      flex: 1;
      .count-title {
        margin-bottom: 20px;
        font-size: 14px;
        letter-spacing: 1px;
      }
    }
  }
  .medical-middle {
    margin-top: 30px;
    background-size: 100% 100%;
    height: 220px;
    position: relative;
  }
  .medLeftChart {
    float: left;
    width: 28%;
    height: 100%;
  }
  .medRightChart {
    float: left;
    width: 28%;
    height: 100%;
  }
  .chartMedTxt {
    float: left;
    width: 20%;
    margin-top: 40px;
    color: #e4e4e4;
    .ringLine {
      margin-top: 10px;
      .dot {
        &::before {
          content: '';
          display: inline-block;
          width: 6px;
          height: 6px;
          border-radius: 50%;
          margin-right: 8px;
          margin-bottom: 2px;
        }
      }
      .dotNum {
        margin-left: 12px;
      }
    }
  }
  .medical-bottom {
    margin-top: 30px;
    flex: 1;
    background-size: 100% 100%;
    display: flex;
    position: relative;
  }
  .publicLeftChart {
    height: 100%;
    width: 50%;
  }
  .publicRightList {
    height: 100%;
    width: 48%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    .publicItem {
      display: flex;
      align-items: flex-end;
      justify-content: space-between;
      height: 40px;
      line-height: 40px;
      .files-text {
        flex: 1;
        font-size: 14px;
        &::before {
          content: '';
          display: inline-block;
          width: 6px;
          height: 6px;
          background: #21d849;
          border-radius: 50%;
          margin-right: 8px;
          margin-bottom: 2px;
        }
      }
      .files-num {
        color: #fff;
        font-size: 24px;
        font-weight: 700;
        min-width: 60px;
        max-width: 188px;
        text-align: right;
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-clip: text;
        background-image: -webkit-linear-gradient(
          bottom,
          rgba(4, 89, 247, 0.39),
          #ffffff,
          rgb(255, 255, 255)
        );
      }
    }
  }
  .chartPublicTxt {
    position: absolute;
    top: 30px;
    left: 20px;
    color: #e4e4e4;
    .ringLine {
      .dot {
        &::before {
          content: '';
          display: inline-block;
          width: 6px;
          height: 6px;
          border-radius: 50%;
          margin-right: 8px;
          margin-bottom: 2px;
        }
      }
      .dotNum {
        margin-left: 15px;
      }
    }
  }
  .item-scroll {
    margin-top: 20px;
    padding-right: 10px;
    max-height: 300px;
    overflow: auto;
  }
  ::-webkit-scrollbar-track {
    -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
    border-radius: 10px;
    background-color: #5f5f5f;
  }
  /*定义滑块
 内阴影+圆角*/
  ::-webkit-scrollbar-thumb {
    border-radius: 10px;
    -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
    background-color: #a1a1a1;
  }
  .dotColor1 {
    &::before {
      background: #04d9fe;
    }
  }
  .txtStyle1 {
    color: #04d9fe;
  }
  .dotColor2 {
    &::before {
      background: #bef192;
    }
  }
  .txtStyle2 {
    color: #bef192;
  }
</style>

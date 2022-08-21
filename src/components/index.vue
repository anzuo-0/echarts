<template>
  <div class="wrapper">
    <!-- 搜索框 -->
    <!-- <div class="search-form">
      <el-form
        :inline="true"
        :model="searchForm"
        :rules="searchFormRules"
        ref="searchForm"
      > -->
        <!-- 年份输入框 -->
        <!-- <el-form-item label="年份" prop="year">
          <el-date-picker
            v-model="searchForm.year"
            value-format="yyyy"
            type="year"
            placeholder="请选择年份"
          >
          </el-date-picker>
        </el-form-item> -->
        <!-- 月份输入框 -->
        <!-- <el-form-item label="月份" prop="month">
          <el-input v-model.number="searchForm.month" placeholder="请输入月份">
          </el-input>
        </el-form-item> -->
        <!-- 查询按钮 -->
        <!-- <el-form-item>
          <el-button type="primary" @click="search">查询</el-button>
        </el-form-item>
      </el-form>
    </div> -->
    <!-- echarts视图 -->
    <div>
      <el-row class="el-row-report">
        <el-col class="el-col-report">
          <div ref="chartRisk" class="chartRisk"></div>
        </el-col>
      <el-col class="el-col-report">
          <div ref="chartMap" class="chartMap"></div>
        </el-col>
      </el-row>
      <el-row class="el-row-report">
        <el-col class="el-col-report">
          <div ref="chartWeb" class="chartWeb"></div>
        </el-col>
           <el-col class="el-col-report">
          <div ref="chartDate" class="chartDate"></div>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
import * as echarts from 'echarts'
// import {
//   getMonthMeetingNum,
//   getYearMeetingNum,
//   getMeetingType,
//   getUserType
// } from '@/api/report'

export default {
  data() {
    return {
      loading: false,
      timer: null,
      // 视图
      chartRisk: null,
      chartMap: null,
      chartWeb: null,
      chartDate: null,
      // 数据
      riskData: [],
      mapData: [],
      webTypeData: [],
      dateTypeData: [],
    }
  },
  mounted() {
    // 初始化
    // 风险提示
    this.chartRisk = echarts.init(this.$refs.chartRisk)
    this.chartRisk.setOption(this.optionRisk)
    // this.getYearList()

    // 区域信息
    this.chartMap = echarts.init(this.$refs.chartMap)
    this.chartMap.setOption(this.optionMap)
    // this.getMonthList()

    // 蜘蛛网
    this.chartWeb = echarts.init(this.$refs.chartWeb)
    this.chartWeb.setOption(this.optionWeb)
    // this.getMeetingTypeList()

    // 日期
    this.chartDate = echarts.init(this.$refs.chartDate)
    this.chartDate.setOption(this.optionDate)
    // this.getUserTypeList()
  },
//   methods: {
//     // 年数据列表
//     getYearList() {
//       this.loading = true
//       let list = Array(12).fill(0)
//       getYearMeetingNum(this.searchForm.year).then((res) => {
//         if (res) {
//           this.loading = false
//           for (let i = 0; i < res.data.length; i++) {
//             list[res.data[i].month - 1] = res.data[i].count
//           }
//           this.yearData = list
//           this.chartYear.setOption(this.optionYear)
//         }
//       })
//     },
//     // 月数据列表
//     getMonthList() {
//       this.loading = true
//       let year = parseInt(this.searchForm.year)
//       let month = parseInt(this.searchForm.month)
//       // 在查询一个新的月份记录前，将原来的日期数清空
//       this.monthDays = []
//       getMonthMeetingNum(year, month).then((res) => {
//         if (res) {
//           this.loading = false
//           let days = [0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
//           if ((year % 4 === 0 || year % 100 === 0) && year % 400 === 0) {
//             days[2] = 29
//           } else {
//             days[2] = 28
//           }
//           // 设置指定月的天数
//           for (let i = 0; i < days[month]; i++) {
//             this.monthDays[i] = i + 1
//           }
//           let list = Array(days[month]).fill(0)
//           for (let i = 0; i < res.data.length; i++) {
//             list[res.data[i].day - 1] = res.data[i].count
//           }
//           this.monthData = list
//           this.chartMonth.setOption(this.optionMonth)
//         }
//       })
//     },
//     // 会议类型数据
//     getMeetingTypeList() {
//       getMeetingType().then((res) => {
//         if (res) {
//           this.meetingTypeData = res.data
//           this.chartMeetingType.setOption(this.optionMeetingType)
//         }
//       })
//     },
//     // 用户类型数据
//     getUserTypeList() {
//       getUserType().then((res) => {
//         if (res) {
//           this.userTypeData = res.data
//           this.chartUserType.setOption(this.optionUserType)
//         }
//       })
//     },
//     search() {
//       this.getYearList()
//       this.getMonthList()
//     }
//   },
  computed: {
    // 年
    optionRisk() {
      return {
        title: {
          text: "我的风险提示",
          textStyle: {
            color: '#1f2d3d'
          },
          left: 'center'
        },
        color: ['#304156'],
        xAxis: {
          type: 'category',
          data: [
            '一月',
            '二月',
            '三月',
            '四月',
          ],
          axisTick: {
            //去掉x轴刻度
            show: false
          },
          axisLine: {
            lineStyle: {
              color: '#8595a8'
            }
          },
          name: '月份'
        },
        yAxis: {
          type: 'value',
          name: '会议数量',
          //去除网格线
          splitLine: { 
            show: false 
          },
          axisLine: {
            lineStyle: {
              color: '#8595a8'
            }
          },
          axisTick: {
            show: false
          },
          //最小刻度设置为1
          minInterval: 1 
        },
        series: [
          {
            data: this.yearData,
            type: 'bar',
            barwidth: '20%'
          }
        ],
        tooltip: {
          trigger: 'item'
        }
      }
    },
    // 月
    optionMap() {
      return {
        title: {
          text: this.monthTitle,
          textStyle: {
            color: '#1f2d3d'
          },
          left: 'center'
        },
        grid: {
          top: '20%',
          left: '6%',
          right: '8%',
          bottom: '3%',
          containLabel: true
        },
        xAxis: {
          type: 'category',
          data: this.monthDays,
          boundaryGap: false,
          axisLine: {
            lineStyle: {
              color: '#8595a8'
            }
          },
          axisTick: {
            //去掉x轴刻度
            show: false
          },
          name: '日期'
        },
        yAxis: {
          type: 'value',
          name: '会议数量',
          axisLine: {
            lineStyle: {
              color: '#8595a8'
            }
          },
          axisTick: {
            show: false
          },
          //去除网格线
          splitLine: { 
            show: false 
          },
          //最小刻度设置为1
          minInterval: 1 
        },
        series: [
          {
            data: this.monthData,
            type: 'line',
            lineStyle: {
              color: '#004C63'
            },
            itemStyle: {
              color: '#004C63'
            },
            //标记的图形（拐点）
            symbol: 'circle', 
            //大小
            symbolSize: 5 
          }
        ],
        tooltip: {
          // axis item none三个值
          trigger: 'axis' 
        }
      }
    },
    // 会议类型
    optionWeb() {
      return {
        title: {
          subtext: '会议类型',
          left: 'center',
          top: '43%',
          subtextStyle: {
            fontSize: 18
          }
        },
        tooltip: {
          trigger: 'item'
        },
        legend: {
          icon: 'circle',
          top: '0',
          left: 'right'
        },
        series: [
          {
            name: '会议数量',
            type: 'pie',
            radius: ['40%', '55%'],
            label: {
              show: true,
              padding: [0, -60],
              overflow: 'none',
              fontSize: '15',
              fontWeight: 'bold',
              formatter: '{d}%\n\n{c}'
            },
            labelLine: {
              show: true,
              length: 15,
              length2: 60
            },
            itemStyle: {
              normal: {
                color: function (params) {
                  var colorList = ['#3f51b5', '#ffcb89']
                  return colorList[params.dataIndex]
                }
              }
            },
            data: this.meetingTypeData
          }
        ]
      }
    },
    // 用户类型
    optionDate() {
      return {
        title: {
          subtext: '用户类型',
          left: 'center',
          top: '43%',
          subtextStyle: {
            fontSize: 18
          }
        },
        tooltip: {
          trigger: 'item'
        },
        legend: {
          icon: 'circle',
          top: '0',
          left: 'right'
        },
        series: [
          {
            name: '用户数量',
            type: 'pie',
            radius: ['40%', '55%'],
            label: {
              show: true,
              padding: [0, -60],
              overflow: 'none',
              fontSize: '15',
              fontWeight: 'bold',
              formatter: '{d}%\n\n{c}'
            },
            labelLine: {
              show: true,
              length: 15,
              length2: 60
            },
            itemStyle: {
              normal: {
                color: function (params) {
                  var colorList = ['#005ea1', '#45c0ff', '#ffcb89']
                  return colorList[params.dataIndex]
                }
              }
            },
            data: this.userTypeData
          }
        ]
      }
    },
    // echarts标题
    yearTitle() {
      return this.searchForm.year + '年会议统计'
    },
    monthTitle() {
      return this.searchForm.year + '年' + this.searchForm.month + '月会议统计'
    }
  },
  destroyed() {
    clearInterval(this.timer)
  }
}
</script>

<style>
.wrapper{
  background: linear-gradient(to top #8dc9f1, 4160d2);
}
.chartRisk{
  width: 100%;
  height: 100%;
}
.chartMap{
  width: 100%;
  height: 100%;
}
.chartWeb {
  width: 100%;
  height: 100%;
}
.chartDate {
  width: 100%;
  height: 100%;
}
.el-row-report {
  width: 100%;
  height: auto;
  display: flex;
}
.el-col-report {
  height: 300px;
  margin-top: 10px;
  margin-right: 20px;
  flex: 1;
}
</style>

<!-- 运营数据看板 -->
<template>
  <div class="average-time">
    <div class="average-title">
      <title-com title="运输平均时长"></title-com>
      <el-radio-group v-model="earthTab" size="small" class="ranking-tab" @change="changeEarthTab">
        <el-radio-button :label="'day'">近7日</el-radio-button>
        <el-radio-button :label="'month'">近30个月</el-radio-button>
      </el-radio-group>
    </div>

    <div class="bar-chart mt-10">
      <syzk-pie-chart :options="options" :myChart="myChart"></syzk-pie-chart>
    </div>
  </div>
</template>

<script>
import { ref, toRefs, reactive, inject, onMounted } from 'vue'
// import TitleCom from './components/TitleCom'

export default {
  // components: { TitleCom },
  props: {
    color: {
      type: Array,
      default: () => {
        return ['#006ced', '#00cfff', '#ffe000', '#ff5b00', '#ff3000', '#660000']
      }
    }
  },

  setup(props) {
    const t = inject('t')
    const state = reactive({
      myChart: ref(''),
      timer: ref(''),
      earthTab: 'day',
      data: [],
      trafficWay1: [
        {
          name: '空返',
          value: 58000
        },
        {
          name: '卸载',
          value: 134000
        },
        {
          name: '卸载等待',
          value: 138000
        },
        {
          name: '运输',
          value: 218000
        },
        {
          name: '装载等待',
          value: 140000
        },
        {
          name: '装载',
          value: 140000
        }
      ],
      options: {
        backgroundColor: '#04033a',
        color: props.color,
        series: []
      }
    })

    onMounted(() => {
      for (let i = 0; i < state.trafficWay1.length; i++) {
        state.data.push(
          {
            value: state.trafficWay1[i].value,
            name: state.trafficWay1[i].name,
            itemStyle: {
              normal: {
                borderWidth: 5,
                shadowBlur: 10,
                borderColor: props.color[i],
                shadowColor: props.color[i]
              }
            }
          },
          {
            value: 2,
            name: '',
            itemStyle: {
              normal: {
                label: {
                  show: false
                },
                labelLine: {
                  show: false
                },
                color: 'rgba(0, 0, 0, 0)',
                borderColor: 'rgba(0, 0, 0, 0)',
                borderWidth: 0
              }
            }
          }
        )
      }

      const timeFilter = val => {
        function p(t) {
          return t < 10 ? '0' + t : t
        }

        let m = Math.floor((val / 1000 / 60) % 60)
        let s = Math.floor((val / 1000) % 60)
        let str = p(m) + '分' + p(s) + '秒'
        return str
      }
      state.options.series = [
        //安排出运
        {
          name: '',
          type: 'pie',
          clockWise: false,
          radius: ['50%', '40%'],
          hoverAnimation: false,
          itemStyle: {
            normal: {
              label: {
                show: true,
                position: 'outside',
                color: '#c1cadf',
                formatter: function (params) {
                  let percent = 0
                  percent = timeFilter(params.value)
                  if (params.name !== '') {
                    return params.name + '\n' + ':' + percent
                  } else {
                    return ''
                  }
                }
              },
              labelLine: {
                length: '5%',
                length2: '6%',
                show: true,
                color: '#00ffff'
              }
            }
          },
          data: state.data
        }
      ]
    })

    const changeEarthTab = () => {
      console.log('运输平均时长点击时间切换--->', '切换页签重新请求数据')
    }

    return {
      ...toRefs(state),
      changeEarthTab
    }
  }
}
</script>

<style lang="scss" scoped>
.average-time {
  width: 100%;
  height: calc(100% - 4rem);
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  padding-bottom: 4rem;
  box-sizing: border-box;
  .average-title {
    position: relative;
    .ranking-tab {
      position: absolute;
      top: 0.7rem;
      left: 50%;
    }
  }
  .bar-chart {
    border-radius: 1rem;
    height: calc(100% - 4rem);
    width: 85%;
  }
}
</style>

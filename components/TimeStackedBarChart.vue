<template>
  <data-view
    :title="title"
    :title-id="titleId"
    :date="date"
    :url="url"
    :remarks="remarks"
  >
    <template v-slot:button>
      <!-- <p class="Graph-Desc">
        （注）同一の対象者について複数の検体を調査する場合あり
      </p> -->
    </template>
    <bar
      :chart-id="chartId"
      :chart-data="displayData"
      :options="options"
      :height="240"
    />
    <div>
      <ul class="remarks">
        <li v-for="remarks_text in remarks" :key="remarks_text">
          {{ remarks_text }}
        </li>
      </ul>
    </div>
    <template v-slot:infoPanel>
      <data-view-basic-info-panel
        :l-text="displayInfo.lText"
        :l-title="displayInfo.lTitle"
        :s-text="displayInfo.sText"
        :unit="displayInfo.unit"
      />
    </template>
  </data-view>
</template>

<script>
import DataView from '@/components/DataView.vue'
import DataViewBasicInfoPanel from '@/components/DataViewBasicInfoPanel.vue'

export default {
  components: { DataView, DataViewBasicInfoPanel },
  props: {
    title: {
      type: String,
      required: false,
      default: ''
    },
    titleId: {
      type: String,
      required: false,
      default: ''
    },
    chartId: {
      type: String,
      required: false,
      default: 'time-stacked-bar-chart'
    },
    chartData: {
      type: Array,
      required: true,
      default: () => []
    },
    chartLegends: {
      type: Array,
      required: true,
      default: () => []
    },
    date: {
      type: String,
      required: true,
      default: ''
    },
    latestValueField: {
      type: String,
      required: true,
      default: ''
    },
    latestValueTitle: {
      type: String,
      required: false,
      default: ''
    },
    // items: {
    //   type: Array,
    //   required: false,
    //   default: () => []
    // },
    // labels: {
    //   type: Array,
    //   required: false,
    //   default: () => []
    // },
    unit: {
      type: String,
      required: false,
      default: ''
    },
    url: {
      type: String,
      required: false,
      default: ''
    },
    remarks: {
      type: Array,
      required: false,
      default: () => []
    }
  },
  // data() {
  //   return {
  //     dataKind: 'transition'
  //   }
  // },
  computed: {
    displayLatestValueRatio() {
      const lastDay = this.chartData.slice(-1)[0][this.latestValueField]
      const lastDayBefore = this.chartData.slice(-2)[0][this.latestValueField]
      return lastDay && lastDayBefore
        ? this.formatDayBeforeRatio(lastDay - lastDayBefore)
        : '-'
    },
    displayInfo() {
      return {
        lText:
          this.chartData
            .slice(-1)[0]
            [this.latestValueField]?.toLocaleString() ?? '-',
        lTitle: this.latestValueTitle,
        sText: `${this.chartData.slice(-1)[0].label} 実績値（前日比：${
          this.displayLatestValueRatio
        } ${this.unit}）`,
        unit: this.unit
      }
    },
    displayData() {
      return {
        labels: this.chartData.map(d => {
          return d.label
        }),
        datasets: this.chartLegends.map(legend => ({
          label: legend.label,
          data: this.chartData.map(d => d[legend.field]),
          borderWidth: 0,
          borderColor: 'white',
          backgroundColor: legend.backgroundColor
        }))
      }
    },
    options() {
      return {
        responsive: true,
        maintainAspectRatio: false,
        legend: {
          display: true
        },
        scales: {
          xAxes: [
            {
              id: 'day',
              stacked: true,
              gridLines: {
                display: false
              },
              ticks: {
                fontSize: 9,
                maxTicksLimit: 20,
                fontColor: '#808080',
                maxRotation: 0,
                minRotation: 0,
                callback: label => {
                  return label.split('/')[1]
                }
              }
            },
            {
              id: 'month',
              stacked: true,
              gridLines: {
                drawOnChartArea: false,
                drawTicks: true,
                drawBorder: false,
                tickMarkLength: 10
              },
              ticks: {
                fontSize: 11,
                fontColor: '#808080',
                padding: 3,
                fontStyle: 'bold',
                callback: label => {
                  const monthStringArry = [
                    'Jan',
                    'Feb',
                    'Mar',
                    'Apr',
                    'May',
                    'Jun',
                    'Jul',
                    'Aug',
                    'Sep',
                    'Oct',
                    'Nov',
                    'Dec'
                  ]
                  const month = monthStringArry.indexOf(label.split(' ')[0]) + 1
                  return month + '月'
                }
              },
              type: 'time',
              time: {
                unit: 'month'
              }
            }
          ],
          yAxes: [
            {
              location: 'bottom',
              stacked: true,
              gridLines: {
                display: true,
                color: '#E5E5E5'
              },
              ticks: {
                suggestedMin: 0,
                maxTicksLimit: 8,
                fontColor: '#808080'
              }
            }
          ]
        }
      }
    }
  },
  methods: {
    formatDayBeforeRatio(dayBeforeRatio) {
      const dayBeforeRatioLocaleString = dayBeforeRatio.toLocaleString()
      switch (Math.sign(dayBeforeRatio)) {
        case 1:
          return `+${dayBeforeRatioLocaleString}`
        case -1:
          return `${dayBeforeRatioLocaleString}`
        default:
          return `${dayBeforeRatioLocaleString}`
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.Graph-Desc {
  margin: 10px 0;
  font-size: 12px;
  color: $gray-3;
}

ul.remarks {
  list-style-type: '※ ';
}
</style>

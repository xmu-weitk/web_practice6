<template>
  <el-card class="introduce">
    <div class="order">
      <el-card 
        class="order-item"
        v-for="item in oderCard" 
        :key="item.id"
      >
        <template #header>
          <div class="card-header">
            <span>{{item.cardHeader}}</span>
          </div>
        </template>
        <div class="item">{{item.cardItem}}</div>
      </el-card>
    </div>
    <div id="chart"></div>
  </el-card>
</template>

<script>
import * as echarts from 'echarts';
export default {
  data() {
    return{
      oderCard: [
        {cardHeader: '今日订单数', cardItem: '1888'},
        {cardHeader: '今日日活', cardItem: '36271'},
        {cardHeader: '转化率', cardItem: '20%'}
      ]
    }
  },
  mounted() {
    this.chartDraw()
  },
  methods:{
    chartDraw() {
      const myChart = echarts.init(document.getElementById('chart'));
      var option = {
          title: {
            text: '系统折线图'
          },
          tooltip: {
            trigger: 'axis',
            axisPointer: {
              type: 'cross',
              label: {
                backgroundColor: '#6a7985'
              }
            }
          },
          legend: {
            data: ['新增注册', '付费用户', '活跃用户', '订单数', '当日总收入']
          },
          toolbox: {
            feature: {
              saveAsImage: {}
            }
          },
          grid: {
            left: '3%',
            right: '4%',
            bottom: '3%',
            containLabel: true
          },
          xAxis: [
            {
              type: 'category',
              boundaryGap: false,
              data: ['2021-03-11', '2021-03-12', '2021-03-13', '2021-03-14', '2021-03-15', '2021-03-16', '2021-03-17']
            }
          ],
          yAxis: [
            {
              type: 'value'
            }
          ],
          series: [
            {
              name: '新增注册',
              type: 'line',
              stack: '总量',
              areaStyle: {},
              emphasis: {
                focus: 'series'
              },
              data: [120, 132, 101, 134, 90, 230, 210]
            },
            {
              name: '付费用户',
              type: 'line',
              stack: '总量',
              areaStyle: {},
              emphasis: {
                focus: 'series'
              },
              data: [220, 182, 191, 234, 290, 330, 310]
            },
            {
              name: '活跃用户',
              type: 'line',
              stack: '总量',
              areaStyle: {},
              emphasis: {
                focus: 'series'
              },
              data: [150, 232, 201, 154, 190, 330, 410]
            },
            {
              name: '订单数',
              type: 'line',
              stack: '总量',
              areaStyle: {},
              emphasis: {
                focus: 'series'
              },
              data: [320, 332, 301, 334, 390, 330, 320]
            },
            {
              name: '当日总收入',
              type: 'line',
              stack: '总量',
              label: {
                show: true,
                position: 'top'
              },
              areaStyle: {},
              emphasis: {
                focus: 'series'
              },
              data: [820, 932, 901, 934, 1290, 1330, 1320]
            }
          ]
        };
        myChart.setOption(option);
    }
  }
}
</script>

<style scoped>
  .introduce .order {
    display: flex;
    margin-bottom: 50px;
  }
  .introduce .order .order-item {
    flex: 1;
    margin-right: 20px;
  }
  .introduce .order .order-item:last-child{
    margin-right: 0;
  }
  #chart {
    min-height: 300px;
    width: 100%;
  }
</style>
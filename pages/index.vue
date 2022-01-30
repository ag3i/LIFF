<template>
  <div class="container">
    <div style="text-align: center">
      <el-row>
        <el-button type="warning" round>AWS</el-button>
        <el-button type="success" round>GCP</el-button>
        <el-button type="primary" round>Azure</el-button>
      </el-row> 
    </div>
      <chart 
        :chartType="chartType"
        :chartData="chartData"
        :chartOptions="chartOptions"
      />
      <div style="text-align: center">
        <el-button  type="danger" icon="el-icon-close" circle @click="handleCancel()"></el-button>
      </div>
      <br>
  </div>
</template>

<script>
import Chart from '@/components/chart.vue'
export default {
  
  components: {
    Chart,
  },
  
  data() {
    return {
      formData: {
        name: ''
      },
      lineId: null,

      chartType: 'PieChart',
      chartData: [
        ['Service', 'Cost',],
        ['ECS', 1170],
        ['EC2', 1000],
        ['EK1', 1055],
        ['EK1', 1054],
        ['EKS', 1030],
        ['LAMDBA', 760],
        ['Other', 300],
        
      ],
      chartOptions: {
        width: 700,
        height: 700,
        pieHole: 0.4,
        backgroundColor: '#EBEEF5',
        chartArea:{left:10,top:0},
        legend: 'none',
        pieSliceText: 'label',
        pieSliceTextStyle: {
            color: 'black',
          },
        slices: {
          0: { color: '#FFD700' },
          1: { color: '#3399FF' },
          2: { color: '#606266' },
          3: { color: '#797B80' },
          4: { color: '#C0C4CC' },
          5: { color: '#DCDFE6' },
          6: { color: '#E4E7ED' }
        }
      }
    }
  },
  mounted() {  
    if (!this.canUseLIFF()) {
      return
    }
    window.liff.init(data => {
      this.lineId = data.context.userId || null
    })
  },
  methods: {
    handleCancel() {
      window.liff.closeWindow()
    },
    canUseLIFF() {
      return navigator.userAgent.indexOf('Line') !== -1 && window.liff
    }
  }
}
</script>
<style >
.container {
  background-color:#EBEEF5;
} 
</style>
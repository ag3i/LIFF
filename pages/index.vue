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
        <el-button type="danger" icon="el-icon-close" circle></el-button>
      </div>
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
        ['EC2', 1000],
        ['ECS', 1170],
        ['LAMDBA', 660],
        ['EKS', 1030],
        ['EK1', 1054],
        ['EK1', 1054],
        ['EK1', 1054],
        
      ],
      chartOptions: {
        width: 450,
        height: 450,
        pieHole: 0.5,
        pieSliceTextStyle: {
            color: 'black',
          },
        sliceVisibilityThreshold: .1,
        slices: {
          0: { color: '#112036' },
          1: { color: 'transparent' }
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
      if (!liff.isInClient()) {
        window.alert('This button is unavailable as LIFF is currently being opened in an external browser.');
      } else {
        liff.closeWindow();
  }
    },
    canUseLIFF() {
      return navigator.userAgent.indexOf('Line') !== -1 && window.liff
    }
  }
}
</script>
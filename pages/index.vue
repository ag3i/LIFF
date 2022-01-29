<template>
  <div class="container">
    <el-row>
      <el-button type="warning" round>AWS</el-button>
      <el-button type="success" round>GCP</el-button>
      <el-button type="primary" round>Azure</el-button>
    </el-row>  
      <chart
        :chartType="chartType"
        :chartData="chartData"
        :chartOptions="chartOptions"
      />
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
        ['年', '売上',],
        ['2014', 1000],
        ['2015', 1170],
        ['2016', 660],
        ['2017', 1030]
      ],
      chartOptions: {
        title: '会社の損益',
        subtitle: '売上',
        width: 500,
        height: 500,
        pieHole: 0.4,
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
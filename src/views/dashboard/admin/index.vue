<template>
  <div class="dashboard-editor-container">

    <el-row :gutter="12">
      <el-col :xs="{span: 24}" :sm="{span: 24}" :md="{span: 24}" :lg="{span: 16}" :xl="{span: 16}" style="padding-right:8px;margin-bottom:30px;">
        <summary-table />
      </el-col>
      <el-col :xs="24" :sm="24" :md="24" :lg="8" :xl="8">
        <el-row>
          <div class="chart-wrapper">
            <UIDInfo />
          </div>
        </el-row>
        <el-row>
          <div class="chart-wrapper">
            <host-name />
          </div>
        </el-row>
        <el-row>
          <div class="chart-wrapper chart">
            <pie-chart />
          </div>
        </el-row>
      </el-col>
    </el-row>

    <el-row>
      <el-col :xs="{span: 24}" :sm="{span: 24}" :md="{span: 24}" :lg="{span: 16}" :xl="{span: 16}" style="padding-right:8px;margin-bottom:30px;">
        <device-table />
      </el-col>
    </el-row>

  </div>
</template>

<script>
import PieChart from './components/PieChart'
import SummaryTable from './components/SummaryTable'
import DeviceTable from './components/DeviceTable'
import UIDInfo from './components/UIDInfo'
import HostName from './components/HostName'

export default {
  name: 'DashboardAdmin',
  components: {
    DeviceTable,
    PieChart,
    SummaryTable,
    UIDInfo,
    HostName
  },
  data() {
    return {
    }
  },
  methods: {
  }
}
</script>

<style lang="scss" scoped>
  .dashboard-editor-container {
    padding: 32px;
    background-color: rgb(240, 242, 245);
    position: relative;

    .chart {
      height: 255px;
    }
    .chart-wrapper {
      background: #fff;
      padding: 16px 16px 0;
      margin-bottom: 16px;
    }
  }

  @media (max-width:1024px) {
    .chart-wrapper {
      padding: 8px;
    }
  }
</style>

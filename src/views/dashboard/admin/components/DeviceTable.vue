<template>
  <div class="infoBox">
    <div class="infoBoxHeader">
      <svg-icon icon-class="documentation" />
      <span>{{ $t('dashboard.deviceInfoTitle') }}</span>
    </div>
    <el-table :data="list" style="width: 100%;padding-top: 10px;">
      <el-table-column :label="$t('dashboard.deviceHeadName')" width="60">
        <template slot-scope="scope">
          {{ scope.row.DeviceTypeName }}
        </template>
      </el-table-column>
      <el-table-column :label="$t('dashboard.deviceHeadNum')" width="80" align="center">
        <template slot-scope="scope">
          {{ scope.row.DeviceIndex }}
        </template>
      </el-table-column>
      <el-table-column :label="$t('dashboard.deviceHeadStatus')" width="80" align="center">
        <template slot-scope="scope">
          {{ scope.row.DevicePresentStatus === 0 ? $t('dashboard.deviceHeadStatus0') : $t('dashboard.deviceHeadStatus1') }}
        </template>
      </el-table-column>
      <el-table-column :label="$t('dashboard.deviceHeadDetail')" min-width="250">
        <template slot-scope="scope">
          {{ scope.row.DeviceDetail }}
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import { transactionList } from '@/api/remote-search'

export default {
  filters: {
    statusFilter(status) {
      const statusMap = {
        success: 'success',
        pending: 'danger'
      }
      return statusMap[status]
    },
    orderNoFilter(str) {
      return str.substring(0, 30)
    }
  },
  data() {
    return {
      list: [
        {
          'DeviceTypeName': 'CPU',
          'DeviceIndex': 0,
          'DevicePresentStatus': 0,
          'DeviceDetail': '4 Cores 2.4GHz AMD EPYC 7351 16-Core Processor  0'
        },
        {
          'DeviceTypeName': 'CPU',
          'DeviceIndex': 1,
          'DevicePresentStatus': 1,
          'DeviceDetail': '4 Cores 2.4GHz AMD EPYC 7351 16-Core Processor  1'
        },
        {
          'DeviceTypeName': 'CPU',
          'DeviceIndex': 2,
          'DevicePresentStatus': 1,
          'DeviceDetail': '4 Cores 2.4GHz AMD EPYC 7351 16-Core Processor  2'
        },
        {
          'DeviceTypeName': 'DIMM',
          'DeviceIndex': 0,
          'DevicePresentStatus': 1,
          'DeviceDetail': 'CPU0 ChannelA0 DDR4 2400MHz 16GB Samsung PN:M393A1G40DB0-CPB SN:01DAB3DB'
        },
        {
          'DeviceTypeName': 'DIMM',
          'DeviceIndex': 1,
          'DevicePresentStatus': 1,
          'DeviceDetail': 'CPU0 ChannelA0 DDR4 2400MHz 16GB Samsung PN:M393A1G40DB0-CPB SN:01DAB3DB'
        },
        {
          'DeviceTypeName': 'DIMM',
          'DeviceIndex': 2,
          'DevicePresentStatus': 1,
          'DeviceDetail': 'CPU0 ChannelA0 DDR4 2400MHz 16GB Samsung PN:M393A1G40DB0-CPB SN:01DAB3DB'
        },
        {
          'DeviceTypeName': 'HDD',
          'DeviceIndex': 0,
          'DevicePresentStatus': 1,
          'DeviceDetail': '4000GB AHCI Kingston SV300S37A120G SN:AA17105827014275 FW:p0617a'
        },
        {
          'DeviceTypeName': 'HDD',
          'DeviceIndex': 1,
          'DevicePresentStatus': 1,
          'DeviceDetail': '4000GB AHCI Kingston SV300S37A120G SN:AA17105827014275 FW:p0617a'
        },
        {
          'DeviceTypeName': 'HDD',
          'DeviceIndex': 2,
          'DevicePresentStatus': 1,
          'DeviceDetail': '4000GB AHCI Kingston SV300S37A120G SN:AA17105827014275 FW:p0617a'
        }
      ]
    }
  },
  created() {
    // this.fetchData()
  },
  methods: {
    // fetchData() {
    //   transactionList().then(response => {
    //     this.list = response.data.items.slice(0, 8)
    //   })
    // }
  }
}
</script>

<style lang="scss" scoped>
  .infoBox {
    padding: 15px;
    background-color: #fff;
  }
  .infoBoxHeader {
    display: inline-block;
    padding-left: 5px;
    span {
      padding-left: 12px;
      font-size: 20px;
      font-weight: 600;
    }
  }
</style>

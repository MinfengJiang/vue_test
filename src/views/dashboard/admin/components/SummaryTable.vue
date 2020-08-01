<template>
  <div class="infoBox">
    <div class="infoBoxHeader">
      <svg-icon icon-class="documentation" />
      <span>{{ $t('dashboard.summaryInfoTitle') }}</span>
    </div>
    <el-table :data="list" style="width: 100%;padding-top: 10px;">
      <el-table-column :label="$t('dashboard.summaryHeadName')" min-width="200">
        <template slot-scope="scope">
          {{ scope.row.Name }}
        </template>
      </el-table-column>
      <el-table-column :label="$t('dashboard.summaryHeadInfo')" width="200" align="center">
        <template slot-scope="scope">
          {{ scope.row.Data.Revision }}
        </template>
      </el-table-column>
      <el-table-column :label="$t('dashboard.summaryHeadData')" width="200" align="center">
        <template slot-scope="scope">
          {{ scope.row.Data.Value }}
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
// import { transactionList } from '@/api/remote-search'

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
          'Name': 'BMC Firmware Information',
          'Status': 0,
          'Data': {
            'Revision': '3.0.0',
            'Value': '20190806'
          }
        },
        {
          'Name': 'BIOS Firmware Information',
          'Status': 0,
          'Data': {
            'Revision': '',
            'Value': 'Unavailable'
          }
        },
        {
          'Name': 'CPLD Firmware Information',
          'Status': 0,
          'Data': {
            'Revision': '3.0.0',
            'Value': 'Unavailable'
          }
        },
        {
          'Name': 'Device UUID',
          'Status': 0,
          'Data': {
            // 'Revision': '3.0.0',
            'Value': '9e3299de6d05cd8838f20f0cef4943fc'
          }
        },
        {
          'Name': 'Product Name',
          'Status': 0,
          'Data': {
            'Revision': '3.0.0',
            'Value': 'ProductName Define'
          }
        },
        {
          'Name': 'Manufacturer Name',
          'Status': 0,
          'Data': {
            'Revision': '3.0.0',
            'Value': 'Manufacturer Define'
          }
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

<template>
  <div class="app-container">
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('deviceChangeLog.deviceChangeLogTitle') }}</span>
          <el-button type="primary" plain size="mini">{{ $t('deviceChangeLog.btn1') }}</el-button>
        </div>
        <el-table :data="list" style="width: 100%;padding-top: 10px;" :default-sort="{prop: 'date', order: 'descending'}">
          <el-table-column :label="$t('deviceChangeLog.Num')" width="60" prop="id" sortable>
            <template slot-scope="scope">{{ scope.row.Num }}</template>
          </el-table-column>
          <el-table-column :label="$t('deviceChangeLog.Hostname')" width="130">
            <template slot-scope="scope">{{ scope.row.Hostname }}</template>
          </el-table-column>
          <el-table-column :label="$t('deviceChangeLog.UserName')" width="140">
            <template slot-scope="scope">{{ scope.row.UserName }}</template>
          </el-table-column>
          <el-table-column :label="$t('deviceChangeLog.ServiceName')" width="130">
            <template slot-scope="scope">{{ scope.row.ServiceName }}</template>
          </el-table-column>
          <el-table-column :label="$t('deviceChangeLog.Client_ip')" width="160">
            <template slot-scope="scope">{{ scope.row.Client_ip }}</template>
          </el-table-column>
          <el-table-column :label="$t('deviceChangeLog.Operation')" width="130">
            <template slot-scope="scope">{{ scope.row.Operation }}</template>
          </el-table-column>
          <el-table-column :label="$t('deviceChangeLog.Time')">
            <template slot-scope="scope">{{ scope.row.Time }}</template>
          </el-table-column>
        </el-table>
      </div>
    </el-row>
    <el-row class="pageStyle">
      <el-pagination
        :current-page="currentPage4"
        :page-sizes="[100, 200, 300, 400]"
        :page-size="100"
        :layout="paginationLayout"
        :total="1000"
        background
        :small="isSmall"
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
      />
    </el-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      list: [
        {
          'Num': 1,
          'Time': 'Thu Jan  1 00:02:48 1970',
          'Hostname': 'X1',
          'ServiceName': 'web',
          'Client_ip': '192.168.100.40',
          'UserName': 'ADMIN',
          'Operation': 'ClearAuditLog'
        },
        {
          'Num': 2,
          'Time': 'Thu Jan  1 00:11:51 1970',
          'Hostname': 'BMY',
          'ServiceName': 'web',
          'Client_ip': '192.168.100.40',
          'UserName': 'ADMIN',
          'Operation': 'Logout'
        },
        {
          'Num': 3,
          'Time': 'Thu Jan  1 00:12:14 1970',
          'Hostname': '123',
          'ServiceName': 'web',
          'Client_ip': '192.168.100.40',
          'UserName': 'ADMIN',
          'Operation': 'Login'
        },
        {
          'Num': 4,
          'Time': 'Thu Jan  1 00:12:14 1970',
          'Hostname': 'BMC',
          'ServiceName': 'web',
          'Client_ip': '192.168.100.40',
          'UserName': 'ADMIN',
          'Operation': 'Login'
        },
        {
          'Num': 5,
          'Time': 'Thu Jan  1 00:12:14 1970',
          'Hostname': 'jintide',
          'ServiceName': 'web',
          'Client_ip': '192.168.100.40',
          'UserName': 'ADMIN',
          'Operation': 'Login'
        },
        {
          'Num': 6,
          'Time': 'Thu Jan  1 00:12:14 1970',
          'Hostname': 'jintide',
          'ServiceName': 'web',
          'Client_ip': '192.168.100.40',
          'UserName': 'ADMIN',
          'Operation': 'Login'
        }
      ],
      currentPage4: 1,
      paginationLayout: '',
      isSmall: false,
      screenWidth: document.documentElement.clientWidth,
      inputVal: ''
    }
  },
  mounted() {
    this.changePaginationStyle()
    window.addEventListener('resize', () => {
      return (() => {
        this.screenWidth = document.documentElement.clientWidth
        this.changePaginationStyle()
      })()
    }, false)
  },
  methods: {
    changePaginationStyle() {
      // console.log('screenWidth', this.screenWidth)
      if (this.screenWidth <= 400) {
        this.isSmall = true
        this.paginationLayout = 'prev, pager, next'
      } else if (this.screenWidth > 400 && this.screenWidth <= 595) {
        this.isSmall = true
        this.paginationLayout = 'total, prev, pager, next'
      } else if (this.screenWidth > 595 && this.screenWidth <= 715) {
        this.isSmall = false
        this.paginationLayout = 'total, prev, pager, next'
      } else if (this.screenWidth > 715 && this.screenWidth <= 830) {
        this.isSmall = false
        this.paginationLayout = 'total, sizes, prev, pager, next'
      } else {
        this.isSmall = false
        this.paginationLayout = 'total, sizes, prev, pager, next, jumper'
      }
    },
    filterHandler(value, row, column) {
      const property = column['property']
      return row[property] === value
    },
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`)
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`)
    }
  }
}
</script>

<style lang="scss" scoped>
@media screen and (max-width: 465px) {
  .app-container {
    position: absolute;
    height: 100%;
    width: 100%;
    overflow: auto;
    padding: 0;
  }
}
.app-container {
  position: absolute;
  height: 100%;
  width: 100%;
  overflow: auto;
}
.el-row {
  // background-color: rgb(240, 242, 245);
  background-color: #fff;
  min-width: 310px !important;
}
.infoBox {
  padding: 15px;
  background-color: #fff;
}
.infoBoxHeader {
  display: flex;
  span {
    font-size: 18px;
    font-weight: 600;
    flex: 5;
  }
  .el-select {
      flex: 1;
      .el-input--medium .el-input__inner {
        height: 20px;
        line-height: 20px;
    }
  }
  .el-button {
    font-weight: 600;
    letter-spacing: 1px;
    padding: 7px 8px;
  }
}
.pageStyle {
  min-width: 310px !important;
  .el-pagination {
    margin: 10px 15px 20px 0;
    float: right;
  }
}

</style>

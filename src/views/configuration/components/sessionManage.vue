<template>
  <div class="app-container">
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('configuration.sessionManageConfig.sessionManageConfigTitle') }}</span>
        </div>
        <el-table :data="list" style="width: 100%;padding-top: 10px;">
          <el-table-column type="index" :label="$t('configuration.sessionManageConfig.ID')" min-width="60px" />
          <el-table-column :label="$t('configuration.sessionManageConfig.type')" min-width="80px">
            <template slot-scope="scope">{{ scope.row.SessionType }}</template>
          </el-table-column>
          <el-table-column :label="$t('configuration.sessionManageConfig.IP')" align="center" header-align="center">
            <template slot-scope="scope">{{ scope.row.Client_IP }}</template>
          </el-table-column>
          <el-table-column :label="$t('configuration.sessionManageConfig.userName')" align="center" header-align="center">
            <template slot-scope="scope">{{ scope.row.UserName }}</template>
          </el-table-column>
          <el-table-column :label="$t('configuration.sessionManageConfig.time')" align="center" header-align="center" min-width="200px">
            <template slot-scope="scope">{{ scope.row.LastActiveTime }}</template>
          </el-table-column>
          <el-table-column :label="$t('configuration.sessionManageConfig.action')" align="center" header-align="center" min-width="120px">
            <template slot-scope="scope">
              <div>
                <el-button type="danger" size="mini" @click="deleteSessionHandle(scope.row)">{{ $t('configuration.sessionManageConfig.delete') }}</el-button>
              </div>
            </template>
          </el-table-column>
        </el-table>
      </div>
    </el-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      list: [
        {
          'SessionIndex': 1234,
          'SessionType': 'kvm',
          'Client_IP': '192.168.100.40',
          'UserName': 'ADMIN',
          'LastActiveTime': 'Thu Jan 1 00:12:14 1970'
        },
        {
          'SessionIndex': 2686,
          'SessionType': 'web',
          'Client_IP': '192.168.100.40',
          'UserName': 'ADMIN',
          'LastActiveTime': 'Thu Jan 1 00:12:14 1970'
        },
        {
          'SessionIndex': 2548,
          'SessionType': 'ssh',
          'Client_IP': '192.168.100.40',
          'UserName': 'BBB',
          'LastActiveTime': 'Thu Jan 1 00:12:14 1970'
        }
      ]
    }
  },
  computed: {
  },
  mounted() {
  },
  methods: {
    deleteSessionHandle(value) {
      console.log(2223333, value.SessionIndex)
      this.$confirm(this.$t('configuration.sessionManageConfig.comfirmMsg'), this.$t('configuration.sessionManageConfig.comfirmTitle'), {
        confirmButtonText: this.$t('configuration.sessionManageConfig.confirmButtonText'),
        cancelButtonText: this.$t('configuration.sessionManageConfig.cancelButtonText'),
        type: 'warning'
      }).then(() => {
        this.$message({
          type: 'success',
          message: this.$t('configuration.sessionManageConfig.successMsg')
        })
      }).catch(() => {
        this.$message({
          type: 'info',
          message: this.$t('configuration.sessionManageConfig.cancelMsg')
        })
      })
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
.infoBox {
  padding: 15px;
  background-color: #fff;
}
.infoBoxHeader {
  display: inline-block;
  span {
    /*padding-left: 12px;*/
    font-size: 20px;
    font-weight: 600;
  }
}
::v-deep {
  .el-dialog {
    margin-bottom: 10vh;
    margin-top: 5vh !important;
  }
  .el-dialog__header {
    margin-left: 15px;
    // padding: 15px 20px 0 20px;
    font-weight: 600;
  }
  .el-dialog__body {
    padding: 0 20px;
  }
  .el-dialog__footer {
    padding-top: 0;
    text-align: center;
  }
  .el-form-item__label {
    padding-right: 25px;
  }
  .el-input {
    max-width: 250px;
    // min-width: 200px !important;
  }
  // Style adjustment when the default input is number
  input::-webkit-outer-spin-button,
  input::-webkit-inner-spin-button {
      -webkit-appearance: none !important;
  }
  .el-select {
    width: 100%;
  }
}
.userIdStyle {
  // font-size: 16px;
  font-weight: 700;
}
</style>

<template>
  <div class="app-container">
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('configuration.serivceManageConfig.serivceManageConfigTitle') }}</span>
        </div>
        <el-table :data="list" style="width: 100%;padding-top: 10px;">
          <el-table-column type="index" :label="$t('configuration.serivceManageConfig.ID')" width="50px" />
          <el-table-column :label="$t('configuration.serivceManageConfig.service')" align="center" header-align="center" width="100px">
            <template slot-scope="scope">
              <div>
                <div v-if="scope.row.ServiceType === 0" key="ServiceType0">{{ $t('configuration.serivceManageConfig.webService') }}</div>
                <div v-else-if="scope.row.ServiceType === 1" key="ServiceType1">{{ $t('configuration.serivceManageConfig.kvmService') }}</div>
                <div v-else-if="scope.row.ServiceType === 2" key="ServiceType2">{{ $t('configuration.serivceManageConfig.sshService') }}</div>
              </div>
            </template>
          </el-table-column>
          <el-table-column :label="$t('configuration.serivceManageConfig.sessionTimeout')" align="center" header-align="center" min-width="210px">
            <template slot-scope="scope">{{ scope.row.ServiceTimeoutTime }}</template>
          </el-table-column>
          <el-table-column :label="$t('configuration.serivceManageConfig.maxSession')" align="center" header-align="center" min-width="120px">
            <template slot-scope="scope">{{ scope.row.ServiceMaxNum }}</template>
          </el-table-column>
          <el-table-column :label="$t('configuration.serivceManageConfig.serviceSecurityPort')" align="center" header-align="center" min-width="170px">
            <template slot-scope="scope">
              <div v-if="scope.row.ServiceSecurityPort === 65535" key="PortNA">NA</div>
              <div v-else key="Port">{{ scope.row.ServiceSecurityPort }}</div>
            </template>
          </el-table-column>
          <el-table-column :label="$t('configuration.serivceManageConfig.serviceUnSecurePort')" align="center" header-align="center" min-width="190px">
            <template slot-scope="scope">
              <div v-if="scope.row.ServiceNonSecurityPort === 65535" key="PortNA2">NA</div>
              <div v-else key="Port2">{{ scope.row.ServiceNonSecurityPort }}</div>
            </template>
          </el-table-column>
          <el-table-column :label="$t('configuration.serivceManageConfig.action')" align="center" header-align="center" min-width="100px">
            <template slot-scope="scope">
              <div>
                <el-button type="warning" size="mini" @click="editServiceHandle(scope.row)">{{ $t('configuration.serivceManageConfig.modify') }}</el-button>
              </div>
            </template>
          </el-table-column>
        </el-table>
      </div>
    </el-row>
    <el-dialog :title="$t('configuration.serivceManageConfig.modify')" :visible.sync="dialogVisible" :before-close="resetForm">
      <el-form ref="ruleForm" :model="formData" :rules="formRules" :label-width="`${formLabelWidth}px`">
        <el-form-item :label="$t('configuration.serivceManageConfig.service')">
          <div class="userIdStyle">
            <div v-if="formData.Service_Type === 0" key="Type0">{{ $t('configuration.serivceManageConfig.webService') }}</div>
            <div v-else-if="formData.Service_Type === 1" key="Type1">{{ $t('configuration.serivceManageConfig.kvmService') }}</div>
            <div v-else-if="formData.Service_Type === 2" key="Type2">{{ $t('configuration.serivceManageConfig.sshService') }}</div>
          </div>
        </el-form-item>
        <el-form-item>
          <span slot="label">
            {{ $t('configuration.serivceManageConfig.sessionTimeout') }}
            <el-tooltip placement="top-start">
              <div slot="content">{{ $t('configuration.serivceManageConfig.sessionTimeoutHelp') }}</div>
              <i class="el-icon-question" />
            </el-tooltip>
          </span>
          <el-input v-model.number="formData.Service_Timeout_Time" type="number" autocomplete="off" />
        </el-form-item>
        <el-form-item v-if="formData.Service_Security_Port === 65535" key="noProp" :label="$t('configuration.serivceManageConfig.serviceSecurityPort')">
          <el-input value="NA" autocomplete="off" :disabled="formData.Service_Security_Port === 65535" />
        </el-form-item>
        <el-form-item v-else key="hasProp" :label="$t('configuration.serivceManageConfig.serviceSecurityPort')" prop="Service_Security_Port" :rules="formRules.Service_Security_Port">
          <el-input v-model.number="formData.Service_Security_Port" type="number" autocomplete="off" />
        </el-form-item>
        <el-form-item v-if="formData.Service_NonSecurity_Port === 65535" key="noProp2" :label="$t('configuration.serivceManageConfig.serviceUnSecurePort')">
          <el-input value="NA" autocomplete="off" :disabled="formData.Service_NonSecurity_Port === 65535" />
        </el-form-item>
        <el-form-item v-else key="hasProp2" :label="$t('configuration.serivceManageConfig.serviceUnSecurePort')" prop="Service_NonSecurity_Port" :rules="formRules.Service_NonSecurity_Port">
          <el-input v-model.number="formData.Service_NonSecurity_Port" type="number" autocomplete="off" />
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button style="margin-left: -18px" @click="resetForm">{{ $t('configuration.userManageConfig.cancelBtn') }}</el-button>
        <el-button type="primary" style="margin-left: 50px" @click="submitForm">{{ $t('configuration.userManageConfig.confirmBtn') }}</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
export default {
  data() {
    var validateSecurityPort = (rule, value, callback) => {
      if (!Number.isInteger(value)) {
        callback(new Error(this.$t('configuration.serivceManageConfig.validatePortNum')))
      } else if (value < 1 || value >= 65535) {
        callback(new Error(this.$t('configuration.serivceManageConfig.validatePort')))
      } else {
        callback()
      }
    }
    return {
      list: [
        {
          'ServiceType': 0,
          'ServiceTimeoutTime': 1800,
          'ServiceMaxNum': 20,
          'ServiceSecurityPort': 443,
          'ServiceNonSecurityPort': 80
        },
        {
          'ServiceType': 2,
          'ServiceTimeoutTime': 600,
          'ServiceMaxNum': 20,
          'ServiceSecurityPort': 22,
          'ServiceNonSecurityPort': 65535
        },
        {
          'ServiceType': 1,
          'ServiceTimeoutTime': 30,
          'ServiceMaxNum': 1,
          'ServiceSecurityPort': 65535,
          'ServiceNonSecurityPort': ''
        }
      ],
      dialogVisible: false,
      formLabelWidth: document.documentElement.clientWidth * 0.205,
      formData: {
        Service_Timeout_Time: 3200,
        Service_Max_Number: 20,
        Service_Type: 0,
        Service_Security_Port: 443,
        Service_NonSecurity_Port: 80
      },
      currentRowData: {},
      formRules: {
        Service_Security_Port: [
          { validator: validateSecurityPort, trigger: 'blur' }
        ],
        Service_NonSecurity_Port: [
          { validator: validateSecurityPort, trigger: 'blur' }
        ]
      }
    }
  },
  computed: {
  },
  mounted() {
    window.addEventListener('resize', () => {
      return (() => {
        this.formLabelWidth = document.documentElement.clientWidth * 0.205
      })()
    }, false)
  },
  methods: {
    editServiceHandle(value) {
      this.dialogVisible = true
      this.currentRowData = value
      this.formData.Service_Type = value.ServiceType
      this.formData.Service_Timeout_Time = value.ServiceTimeoutTime
      this.formData.Service_Max_Number = value.ServiceMaxNum
      this.formData.Service_Security_Port = value.ServiceSecurityPort
      this.formData.Service_NonSecurity_Port = value.ServiceNonSecurityPort
    },
    submitForm() {
      this.$refs.ruleForm.validate(val => {
        if (!val) return
        if (this.formData.Service_Security_Port !== this.currentRowData.ServiceSecurityPort || this.formData.Service_NonSecurity_Port !== this.currentRowData.ServiceNonSecurityPort) {
          this.$confirm(this.$t('configuration.serivceManageConfig.comfirmMsg'), this.$t('configuration.userManageConfig.comfirmTitle'), {
            confirmButtonText: this.$t('configuration.userManageConfig.confirmButtonText'),
            cancelButtonText: this.$t('configuration.userManageConfig.cancelButtonText'),
            type: 'warning'
          }).then(() => {
            this.$message({
              type: 'success',
              duration: '1000',
              message: this.$t('configuration.timeSettingConfig.successMsg')
            })
            // console.log(111, this.list)
            this.dialogVisible = false
            this.$refs.ruleForm.resetFields()
            setTimeout(async() => {
              await this.$store.dispatch('user/logout')
              this.$router.push('/login?redirect=dashboard')
            }, 1200)
          }).catch(() => {
            this.$message({
              type: 'info',
              message: this.$t('configuration.timeSettingConfig.cancelMsg')
            })
          })
        } else {
          this.dialogVisible = false
          this.$refs.ruleForm.resetFields()
        }
      })
    },
    resetForm() {
      this.dialogVisible = false
      this.$refs.ruleForm.resetFields()
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
  .el-dialog__header {
    margin-left: 15px;
    font-weight: 600;
  }
  .el-dialog__body {
    padding: 0 20px;
  }
  .el-dialog__footer {
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

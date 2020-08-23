<template>
  <div class="app-container">
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('configuration.ipAccessControlConfig.ipAccessControlConfigTitle') }}</span>
        </div>
        <el-table :data="list" style="width: 100%;padding-top: 10px;">
          <el-table-column type="index" :label="$t('configuration.ipAccessControlConfig.ID')" width="40px" />
          <el-table-column :label="$t('configuration.ipAccessControlConfig.IP')" width="240px">
            <template slot-scope="scope">
              <div v-if="scope.row.PolicyEmptyFlag === 1 && scope.row.IPValidFlag === 1" key="IPValidFlag1">{{ scope.row.StartIP }} - {{ scope.row.EndIP }}</div>
              <div v-if="scope.row.PolicyEmptyFlag === 0 || scope.row.IPValidFlag === 0" key="IPValidFlag0" />
            </template>
          </el-table-column>
          <el-table-column :label="$t('configuration.ipAccessControlConfig.MACAddr')">
            <template slot-scope="scope">
              <div v-if="scope.row.PolicyEmptyFlag === 1 && scope.row.MACValidFlag === 1" key="MACValidFlag1">{{ scope.row.MACAddr }}</div>
              <div v-if="scope.row.PolicyEmptyFlag === 0 || scope.row.MACValidFlag === 0" key="MACValidFlag0" />
            </template>
          </el-table-column>
          <el-table-column :label="$t('configuration.ipAccessControlConfig.dateRange')" width="300px">
            <template slot-scope="scope">
              <div v-if="scope.row.PolicyEmptyFlag === 1 && scope.row.TimeValidFlag === 1" key="TimeValidFlag1">
                {{ $moment(scope.row.StartTimeStamp).format('YYYY/MM/DD HH:mm:ss') }} - {{ $moment(scope.row.EndTimeStamp).format('YYYY/MM/DD HH:mm:ss') }}
              </div>
              <div v-if="scope.row.PolicyEmptyFlag === 0 || scope.row.TimeValidFlag === 0" key="TimeValidFlag0" />
            </template>
          </el-table-column>
          <el-table-column :label="$t('configuration.ipAccessControlConfig.rule')" width="60px">
            <template slot-scope="scope">
              <div v-if="scope.row.PolicyEmptyFlag === 1 && scope.row.PolicyAction === 1" key="isShown">
                <span style="color: #67c23a">{{ $t('configuration.ipAccessControlConfig.allow') }}</span>
              </div>
              <div v-if="scope.row.PolicyEmptyFlag === 1 && scope.row.PolicyAction === 0" key="isHidden">
                <span style="color: #f56c6c">{{ $t('configuration.ipAccessControlConfig.deny') }}</span>
              </div>
            </template>
          </el-table-column>
          <el-table-column :label="$t('configuration.ipAccessControlConfig.action')" align="center" header-align="center" width="160px">
            <template slot-scope="scope">
              <div>
                <el-button type="primary" size="mini" :disabled="scope.row.PolicyEmptyFlag !== 0" @click="addAccessHandle(scope.row)">{{ $t('configuration.userManageConfig.add') }}</el-button>
                <el-button type="danger" size="mini" :disabled="scope.row.PolicyEmptyFlag === 0" @click="deleteAccessHandle(scope.row)">{{ $t('configuration.userManageConfig.delete') }}</el-button>
              </div>
            </template>
          </el-table-column>
        </el-table>
      </div>
    </el-row>
    <el-dialog :title="$t('configuration.ipAccessControlConfig.addTitle')" :visible.sync="dialogVisible" :before-close="resetForm" :close-on-click-modal="false">
      <el-form ref="ruleForm" :model="form" :rules="formRules" :label-width="`${formLabelWidth}px`">
        <el-form-item :label="$t('configuration.ipAccessControlConfig.ID')">
          <div class="userIdStyle">{{ form.policy_id }}</div>
        </el-form-item>
        <el-form-item :label="$t('configuration.ipAccessControlConfig.IP')">
          <el-col :span="9">
            <el-form-item prop="start_ip">
              <el-input v-model="form.start_ip" autocomplete="off" />
            </el-form-item>
          </el-col>
          <el-col :span="1" class="ipInputLineStyle">{{ '-' }}</el-col>
          <el-col :span="9">
            <el-form-item prop="end_ip">
              <el-input v-model="form.end_ip" autocomplete="off" />
            </el-form-item>
          </el-col>
        </el-form-item>
        <el-form-item :label="$t('configuration.ipAccessControlConfig.MACAddr')" prop="mac_address">
          <el-input v-model="form.mac_address" autocomplete="off" />
        </el-form-item>
        <el-form-item :label="$t('configuration.ipAccessControlConfig.rule')">
          <el-select v-model="form.policy_action">
            <el-option :label="$t('configuration.ipAccessControlConfig.allow')" :value="1" />
            <el-option :label="$t('configuration.ipAccessControlConfig.deny')" :value="0" />
          </el-select>
        </el-form-item>
        <el-form-item :label="$t('configuration.ipAccessControlConfig.datePolicy')">
          <el-checkbox v-model="form.time_valid_flag" :true-label="1" :false-label="0" />
        </el-form-item>
        <el-form-item :label="$t('configuration.ipAccessControlConfig.dateRangeForm')">
          <el-date-picker
            v-model="dateRange"
            type="datetimerange"
            align="right"
            format="yyyy/MM/dd HH:mm A"
            :clearable="false"
            :start-placeholder="$t('configuration.ipAccessControlConfig.startDate')"
            :end-placeholder="$t('configuration.ipAccessControlConfig.endDate')"
            :default-time="['12:00:00', '08:00:00']"
            :disabled="form.time_valid_flag === 0"
          />
          <!-- <div class="tipStyle">该时间为你电脑的本地时间</div> -->
          <!-- <div class="tipStyle">BMC会转换成BMC本地时间进行设置并显示</div> -->
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
import { validIP, validMac } from '@/utils/validate'

export default {
  data() {
    var validateIPStart = (rule, value, callback) => {
      if (value === '' && this.form.mac_address === '') {
        callback(new Error(this.$t('configuration.ipAccessControlConfig.validateMacAddr')))
      }
      if (value !== '' && !validIP(value)) {
        callback(new Error(this.$t('configuration.ipAccessControlConfig.validateIPErr')))
      } else {
        callback()
      }
    }
    var validateIPEnd = (rule, value, callback) => {
      if (value === '' && this.form.mac_address === '') {
        callback(new Error(this.$t('configuration.ipAccessControlConfig.validateMacAddr')))
      }
      if (value !== '' && value === this.form.start_ip) {
        callback(new Error(this.$t('configuration.ipAccessControlConfig.validateIPErr2')))
      }
      if (value !== '' && !validIP(value)) {
        callback(new Error(this.$t('configuration.ipAccessControlConfig.validateIPErr')))
      } else {
        callback()
      }
    }
    var validateMacAddr = (rule, value, callback) => {
      if (value === '' && this.form.start_ip === '') {
        callback(new Error(this.$t('configuration.ipAccessControlConfig.validateMacAddr')))
      }
      if (value !== '' && !validMac(value)) {
        callback(new Error(this.$t('configuration.ipAccessControlConfig.validateMacAddrErr')))
      } else {
        callback()
      }
    }
    return {
      list: [
        {
          'PolicyID': 1,
          'PolicyEmptyFlag': 1,
          'IPValidFlag': 1,
          'StartIP': '192.168.1.1',
          'EndIP': '192.168.1.5',
          'MACValidFlag': 1,
          'MACAddr': '12:34:56:78:AA:BB',
          'TimeValidFlag': 1,
          'StartTimeStamp': 1525425004,
          'EndTimeStamp': 1525425004,
          'PolicyAction': 1
        },
        {
          'PolicyID': 2,
          'PolicyEmptyFlag': 0,
          'IPValidFlag': 0,
          'StartIP': '0',
          'EndIP': '0',
          'MACValidFlag': 0,
          'MACAddr': '00:00:00:00:00:00',
          'TimeValidFlag': 1,
          'StartTimeStamp': 3,
          'EndTimeStamp': 33333,
          'PolicyAction': 0
        },
        {
          'PolicyID': 3,
          'PolicyEmptyFlag': 0,
          'IPValidFlag': 0,
          'StartIP': '0',
          'EndIP': '0',
          'MACValidFlag': 0,
          'MACAddr': '00:00:00:00:00:00',
          'TimeValidFlag': 0,
          'StartTimeStamp': 0,
          'EndTimeStamp': 0,
          'PolicyAction': 0
        },
        {
          'PolicyID': 4,
          'PolicyEmptyFlag': 0,
          'IPValidFlag': 0,
          'StartIP': '0',
          'EndIP': '0',
          'MACValidFlag': 0,
          'MACAddr': '00:00:00:00:00:00',
          'TimeValidFlag': 0,
          'StartTimeStamp': 0,
          'EndTimeStamp': 0,
          'PolicyAction': 0
        },
        {
          'PolicyID': 5,
          'PolicyEmptyFlag': 0,
          'IPValidFlag': 0,
          'StartIP': '0',
          'EndIP': '0',
          'MACValidFlag': 0,
          'MACAddr': '00:00:00:00:00:00',
          'TimeValidFlag': 0,
          'StartTimeStamp': 0,
          'EndTimeStamp': 0,
          'PolicyAction': 0
        }
      ],
      dialogVisible: false,
      formLabelWidth: document.documentElement.clientWidth * 0.12,
      dateRange: [],
      form: {
        policy_id: 2,
        ip_valid_flag: 0,
        start_ip: '',
        end_ip: '',
        mac_valid_flag: 0,
        mac_address: '',
        time_valid_flag: 0,
        start_time: 0,
        end_time: 0,
        policy_action: 0
      },
      formRules: {
        start_ip: [
          { validator: validateIPStart, trigger: 'change' }
        ],
        end_ip: [
          { validator: validateIPEnd, trigger: 'change' }
        ],
        mac_address: [
          { validator: validateMacAddr, trigger: 'change' }
        ]
      }
    }
  },
  computed: {},
  mounted() {
    window.addEventListener('resize', () => {
      return (() => {
        this.formLabelWidth = document.documentElement.clientWidth * 0.12
      })()
    }, false)
  },
  methods: {
    addAccessHandle(value) {
      this.dialogVisible = true
      this.form.policy_id = value.PolicyID
    },
    submitForm() {
      this.$refs.ruleForm.validate(val => {
        if (!val) return
        if (this.form.time_valid_flag === 0) {
          delete this.form['start_time']
          delete this.form['end_time']
        } else {
          this.form.start_time = this.$moment(this.dateRange[0]).valueOf()
          this.form.end_time = this.$moment(this.dateRange[1]).valueOf()
        }
        this.dialogVisible = false
        this.$refs.ruleForm.resetFields()
      })
    },
    resetForm() {
      this.dialogVisible = false
      this.$refs.ruleForm.resetFields()
    },
    deleteAccessHandle(value) {
      this.$confirm(this.$t('configuration.ipAccessControlConfig.comfirmMsg'), this.$t('configuration.userManageConfig.comfirmTitle'), {
        confirmButtonText: this.$t('configuration.userManageConfig.confirmButtonText'),
        cancelButtonText: this.$t('configuration.userManageConfig.cancelButtonText'),
        type: 'warning'
      }).then(() => {
        this.$message({
          type: 'success',
          message: this.$t('configuration.userManageConfig.successMsg')
        })
        // console.log(value.PolicyID)
      }).catch(() => {
        this.$message({
          type: 'info',
          message: this.$t('configuration.userManageConfig.cancelMsg')
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
    margin-top: 10vh !important;
  }
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
  .el-input--suffix .el-input__inner {
    padding-right: 75px;
  }
  .el-date-editor--datetimerange.el-input, .el-date-editor--datetimerange.el-input__inner {
    max-width: 357px;
    width: 100%;
  }
  .el-date-editor .el-range-input {
    width: 60%;
  }
}
.userIdStyle {
  // font-size: 16px;
  font-weight: 700;
}
.ipInputLineStyle {
  margin: 0 5px;
  text-align: center;
}
.tipStyle {
  font-size: 12px;
  line-height: 20px;
  padding: 5px 100px 0 0;
  color: #999;
}
</style>

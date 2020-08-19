<template>
  <div class="app-container">
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('configuration.ntpSettingConfig.ntpSettingConfigTitle') }}</span>
        </div>
        <div class="labelBox">
          <div class="labelLeft">{{ $t('configuration.ntpSettingConfig.enable') }}</div>
          <div class="labelRight">
            <el-checkbox v-model="checked" />
          </div>
        </div>
        <div class="labelBox">
          <div class="labelLeft">{{ $t('configuration.ntpSettingConfig.OEM') }}</div>
          <div class="labelRight">
            <el-radio v-model="formData.SyncAddrType" :label="0" :disabled="checked">{{ '' }}</el-radio>
          </div>
        </div>
        <div class="labelBox">
          <div class="labelLeft">{{ $t('configuration.ntpSettingConfig.ntpServer') }}</div>
          <div class="labelRight2">
            <el-form ref="ruleForm" :model="formData" :rules="formRules">
              <el-form-item label="" prop="SyncAddr">
                <el-input v-model="formData.SyncAddr" autocomplete="off" :disabled="checked" :placeholder="$t('configuration.ntpSettingConfig.ntpServerPlaceholder')" />
              </el-form-item>
            </el-form>
          </div>
        </div>
      </div>
    </el-row>
    <el-row class="submitForm">
      <el-button type="primary" @click="submitForm">{{ $t('configuration.networkSettingConfig.confirmBtn') }}</el-button>
    </el-row>
  </div>
</template>

<script>
export default {
  data() {
    var validateNTPAddress = (rule, value, callback) => {
      if (value === '') {
        callback(new Error(this.$t('configuration.ntpSettingConfig.ntpServerPlaceholder')))
      } else {
        // 至少一个字符。首字符必须是'a-z','A-Z','0-9','_'，其后字符必须是'a-z','A-Z','0-9', '_', '-', '.'。另外，'$'仅可以用作最后一个字符，不能用作开头。用户名不支持中文字符
        callback()
      }
    }
    return {
      formData: {
        ntp_status: 1,
        SyncAddrType: 0,
        SyncAddr: 'cn.ntp.org.cna'
      },
      checked: false,
      formRules: {
        SyncAddr: [
          { required: true, validator: validateNTPAddress, trigger: 'change' }
        ]
      }
    }
  },
  computed: {
  },
  methods: {
    submitForm() {
      this.$refs.ruleForm.validate(val => {
        if (!val) return
        this.$confirm(this.$t('configuration.ntpSettingConfig.comfirmMsg'), this.$t('configuration.userManageConfig.comfirmTitle'), {
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
    // min-width: 465px;
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
    font-size: 20px;
    font-weight: 600;
  }
}
.labelBox {
  margin: 15px 0;
  font-size: 14px;
  // font-weight: 600;
  color: #666;
  .labelLeft {
    display: inline-block;
    width: 42%;
    text-align: right;
    padding: 10px 35px 10px 20px;
    font-weight: 600;
  }
  .labelRight {
    display: inline-block;
    width: 58%;
    max-width: 250px;
    padding: 10px 0;
    font-weight: 600;
  }
  .labelRight2 {
    display: inline-block;
    width: 58%;
    max-width: 250px;
    padding: 10px 0;
  }
}
.submitForm {
  padding: 0 15px 30px 15px;
  background-color: #fff;
  flex: 1;
  text-align: center;
  padding-left: 6.5%;
}
::v-deep {
  .el-form-item__label {
    padding-right: 35px;
  }
  .el-input {
    max-width: 250px;
    // min-width: 200px !important;
  }
  .el-date-editor.el-input, .el-date-editor.el-input__inner {
    width: 100%;
  }
  .el-input--suffix .el-input__inner {
    padding-right: 75px;
  }
}
</style>

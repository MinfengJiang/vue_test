<template>
  <div class="app-container">
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('security.webSecurity.webSecurityTitle') }}</span>
        </div>
        <div class="labelBox">
          <div class="labelLeft">{{ $t('security.webSecurity.check') }}</div>
          <div class="labelRight">
            <el-form ref="ruleForm" :model="formData" :rules="formRules">
              <el-form-item prop="PreventSwitch">
                <el-checkbox v-model.number="formData.PreventSwitch" :true-label="1" :false-label="2" />
              </el-form-item>
            </el-form>
          </div>
        </div>
        <div class="labelBox">
          <div class="labelLeft">{{ $t('security.webSecurity.maxError') }}</div>
          <div class="labelRight">
            <el-form ref="ruleForm" :model="formData" :rules="formRules">
              <el-form-item prop="MaxErrTimes">
                <el-input v-model.number="formData.MaxErrTimes" type="number" autocomplete="off" :disabled="formData.PwdLenCheckSwitch === 2" :placeholder="$t('security.webSecurity.maxErrorPlaceholder')" />
              </el-form-item>
            </el-form>
          </div>
        </div>
        <div class="labelBox">
          <div class="labelLeft">{{ $t('security.webSecurity.lockTime') }}</div>
          <div class="labelRight">
            <el-form ref="ruleForm" :model="formData" :rules="formRules">
              <el-form-item prop="LockInterval">
                <el-input v-model.number="formData.LockInterval" type="number" autocomplete="off" :disabled="formData.PwdLenCheckSwitch === 2" :placeholder="$t('security.webSecurity.lockTimePlaceholder')" />
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
    var validatePreventSwitch = (rule, value, callback) => {
      if (value === 2) {
        callback(new Error(this.$t('security.webSecurity.tip1')))
      } else {
        callback()
      }
    }
    var validateMaxErrTimes = (rule, value, callback) => {
      if (value === '') {
        callback(new Error(this.$t('security.webSecurity.maxErrorPlaceholder')))
      } else if (value < 1 || value > 255) {
        callback(new Error(this.$t('security.webSecurity.tip3')))
      } else {
        callback()
      }
    }
    var validateLockInterval = (rule, value, callback) => {
      if (value === '') {
        callback(new Error(this.$t('security.webSecurity.lockTimePlaceholder')))
      } else if (value < 1 || value > 65535) {
        callback(new Error(this.$t('security.webSecurity.tip4')))
      } else {
        callback()
      }
    }
    return {
      formData: {
        PreventSwitch: 1,
        MaxErrTimes: 2,
        LockInterval: 3600
      },
      formRules: {
        PreventSwitch: [
          { validator: validatePreventSwitch, trigger: 'change' }
        ],
        MaxErrTimes: [
          { validator: validateMaxErrTimes, trigger: 'change' }
        ],
        LockInterval: [
          { validator: validateLockInterval, trigger: 'change' }
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
        this.$confirm(this.$t('security.webSecurity.comfirmMsg'), this.$t('configuration.userManageConfig.comfirmTitle'), {
          confirmButtonText: this.$t('configuration.userManageConfig.confirmButtonText'),
          cancelButtonText: this.$t('configuration.userManageConfig.cancelButtonText'),
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: this.$t('configuration.timeSettingConfig.successMsg')
          })
        //   console.log(this.formData)
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
  padding-bottom: 20px;
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
  }
}
.submitForm {
  padding: 10px 15px 30px 15px;
  background-color: #fff;
  flex: 1;
  text-align: center;
  padding-left: 6.5%;
}
::v-deep {
  .el-form-item {
    margin-bottom: 5px;
  }
  .el-form-item__label {
    padding-right: 35px;
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
</style>

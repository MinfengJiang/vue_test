<template>
  <div class="app-container">
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('security.passwordSecurity.passwordSecurityTitle') }}</span>
        </div>
        <div class="labelBox">
          <div class="labelLeft">{{ $t('security.passwordSecurity.lenCheck') }}</div>
          <div class="labelRight">
            <el-form ref="ruleForm" :model="formData" :rules="formRules">
              <el-form-item prop="PwdLenCheckSwitch">
                <el-checkbox v-model.number="formData.PwdLenCheckSwitch" :true-label="1" :false-label="2" />
              </el-form-item>
            </el-form>
          </div>
        </div>
        <div class="labelBox">
          <div class="labelLeft">{{ $t('security.passwordSecurity.minLength') }}</div>
          <div class="labelRight">
            <el-form ref="ruleForm" :model="formData" :rules="formRules">
              <el-form-item prop="MiniPwdLen">
                <el-input v-model.number="formData.MiniPwdLen" type="number" autocomplete="off" :disabled="formData.PwdLenCheckSwitch === 2" :placeholder="$t('security.passwordSecurity.minLengthPlaceholder')" />
              </el-form-item>
            </el-form>
          </div>
        </div>
        <div class="labelBox">
          <div class="labelLeft">{{ $t('security.passwordSecurity.maxLength') }}</div>
          <div class="labelRight">
            <el-form ref="ruleForm" :model="formData" :rules="formRules">
              <el-form-item prop="MaxPwdLen">
                <el-input v-model.number="formData.MaxPwdLen" type="number" autocomplete="off" :disabled="formData.PwdLenCheckSwitch === 2" :placeholder="$t('security.passwordSecurity.maxLengthPlaceholder')" />
              </el-form-item>
            </el-form>
          </div>
        </div>
        <div class="labelBox" style="marginTop: 50px">
          <div class="labelLeft">{{ $t('security.passwordSecurity.complexCheck') }}</div>
          <div class="labelRight">
            <el-form ref="ruleForm" :model="formData" :rules="formRules">
              <el-form-item prop="PwdComplexCheckSwitch">
                <el-checkbox v-model.number="formData.PwdComplexCheckSwitch" :true-label="1" :false-label="2" />
              </el-form-item>
            </el-form>
          </div>
        </div>
        <div class="labelBox">
          <div class="labelLeft">{{ $t('security.passwordSecurity.passwordComplex') }}</div>
          <div class="labelRight">
            <el-form ref="ruleForm" :model="formData" :rules="formRules">
              <el-form-item prop="PwdComplexRequire">
                <el-select v-model.number="formData.PwdComplexRequire" :disabled="formData.PwdComplexCheckSwitch === 2">
                  <el-option :label="$t('security.passwordSecurity.complexContent1')" :value="1" />
                  <el-option :label="$t('security.passwordSecurity.complexContent2')" :value="2" />
                  <el-option :label="$t('security.passwordSecurity.complexContent3')" :value="3" />
                </el-select>
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
    var validatePwdLenCheckSwitch = (rule, value, callback) => {
      if (value === 2) {
        callback(new Error(this.$t('security.passwordSecurity.tip1')))
      } else {
        callback()
      }
    }
    var validateMiniPwdLen = (rule, value, callback) => {
      if (value === '') {
        callback(new Error(this.$t('security.passwordSecurity.minLengthPlaceholder')))
      } else if (value < 8 || value > 20) {
        callback(new Error(this.$t('security.passwordSecurity.tip6')))
      } else {
        callback()
      }
    }
    var validateMaxPwdLen = (rule, value, callback) => {
      if (value === '') {
        callback(new Error(this.$t('security.passwordSecurity.maxLengthPlaceholder')))
      } else if (value < 8 || value > 20) {
        callback(new Error(this.$t('security.passwordSecurity.tip7')))
      } else {
        callback()
      }
    }
    var validatePwdComplexCheckSwitch = (rule, value, callback) => {
      if (value === 2) {
        callback(new Error(this.$t('security.passwordSecurity.tip1')))
      } else {
        callback()
      }
    }
    var validatePwdComplexRequire = (rule, value, callback) => {
      if (value === 1) {
        callback(new Error(this.$t('security.passwordSecurity.tip3')))
      } else if (value === 2) {
        callback(new Error(this.$t('security.passwordSecurity.tip4')))
      } else {
        callback()
      }
    }
    return {
      formData: {
        PwdLenCheckSwitch: 1,
        PwdComplexCheckSwitch: 1,
        MiniPwdLen: 10,
        MaxPwdLen: 18,
        PwdComplexRequire: 2
      },
      formRules: {
        PwdLenCheckSwitch: [
          { validator: validatePwdLenCheckSwitch, trigger: 'change' }
        ],
        MiniPwdLen: [
          { validator: validateMiniPwdLen, trigger: 'change' }
        ],
        MaxPwdLen: [
          { validator: validateMaxPwdLen, trigger: 'change' }
        ],
        PwdComplexCheckSwitch: [
          { validator: validatePwdComplexCheckSwitch, trigger: 'change' }
        ],
        PwdComplexRequire: [
          { validator: validatePwdComplexRequire, trigger: 'change' }
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
        this.$confirm(this.$t('security.passwordSecurity.comfirmMsg'), this.$t('configuration.userManageConfig.comfirmTitle'), {
          confirmButtonText: this.$t('configuration.userManageConfig.confirmButtonText'),
          cancelButtonText: this.$t('configuration.userManageConfig.cancelButtonText'),
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            duration: '1000',
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

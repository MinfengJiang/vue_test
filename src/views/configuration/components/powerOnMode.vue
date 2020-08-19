<template>
  <div class="app-container">
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('configuration.powerOnModeConfig.powerOnModeConfigTitle') }}</span>
        </div>
        <div class="labelBox">
          <div class="labelLeft">
            <el-radio v-model="formData.PowerRestorePolicy" :label="2">{{ '' }}</el-radio>
          </div>
          <div class="labelRight">{{ $t('configuration.powerOnModeConfig.serverOn') }}</div>
        </div>
        <div class="labelBox">
          <div class="labelLeft">
            <el-radio v-model="formData.PowerRestorePolicy" :label="0">{{ '' }}</el-radio>
          </div>
          <div class="labelRight">{{ $t('configuration.powerOnModeConfig.serverOff') }}</div>
        </div>
        <div class="labelBox">
          <div class="labelLeft">
            <el-radio v-model="formData.PowerRestorePolicy" :label="1">{{ '' }}</el-radio>
          </div>
          <div class="labelRight">{{ $t('configuration.powerOnModeConfig.beforeDown') }} </div>
        </div>
        <div class="whiteBox" />
        <div class="labelBox">
          <div class="labelLeft">
            <el-radio v-model="formData.PowerOnDelayMode" :label="2">{{ '' }}</el-radio>
          </div>
          <div class="labelRight">{{ $t('configuration.powerOnModeConfig.powerOnRandom') }}</div>
        </div>
        <div class="labelBox">
          <div class="labelLeft">
            <el-radio v-model="formData.PowerOnDelayMode" :label="1">{{ '' }}</el-radio>
          </div>
          <div class="labelRight">{{ $t('configuration.powerOnModeConfig.powerOnDelay') }}</div>
        </div>
        <div class="whiteBox" />
        <div class="labelBox">
          <div class="labelLeft">{{ $t('configuration.powerOnModeConfig.delayTime') }}</div>
          <div class="labelRight2">
            <el-form ref="ruleForm" :model="formData" :rules="formRules">
              <el-form-item label="" prop="PowerOnDelayTime">
                <el-input v-model.number="formData.PowerOnDelayTime" type="number" autocomplete="off" :placeholder="$t('configuration.powerOnModeConfig.powerOnDelayTimePlaceholder')" />
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
    var validatePowerOnDelayTime = (rule, value, callback) => {
      if (value === '') {
        callback(new Error(this.$t('configuration.powerOnModeConfig.powerOnDelayTimePlaceholder')))
      } else {
        callback()
      }
    }
    return {
      formData: {
        PowerRestorePolicy: 0,
        PowerOnDelayMode: 2,
        PowerOnDelayTime: 309
      },
      formRules: {
        PowerOnDelayTime: [
          { validator: validatePowerOnDelayTime, trigger: 'change' }
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
        console.log(typeof this.formData.PowerOnDelayTime, this.formData)
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
.whiteBox{
  width: 100%;
  height: 1px;
  margin: 28px;
  box-sizing: border-box;
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
  // Style adjustment when the default input is number
  input::-webkit-outer-spin-button,
  input::-webkit-inner-spin-button {
      -webkit-appearance: none !important;
  }
  .el-date-editor.el-input, .el-date-editor.el-input__inner {
    width: 100%;
  }
  .el-input--suffix .el-input__inner {
    padding-right: 75px;
  }
}
</style>

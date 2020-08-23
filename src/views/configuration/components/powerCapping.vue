<template>
  <div class="app-container">
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('configuration.powerCappingConfig.powerCappingConfigTitle') }}</span>
        </div>
        <div class="labelBox">
          <div class="labelLeft">{{ $t('configuration.powerCappingConfig.powerCapping') }}</div>
          <div class="labelRight">
            <el-switch
              v-model="formData.PowerCappingSwitch"
              :active-value="1"
              :inactive-value="2"
              style="display: block"
              active-color="#6fc596"
              inactive-color="#ff8080"
              :active-text="$t('configuration.powerCappingConfig.powerEnable')"
              :inactive-text="$t('configuration.powerCappingConfig.powerDisable')"
            />
          </div>
        </div>
        <div class="labelBox">
          <div class="labelLeft2">{{ $t('configuration.powerCappingConfig.powerLimit') }}</div>
          <div class="labelRight">
            <el-input v-model="formData.PowerLimit" type="number" autocomplete="off" :disabled="formData.PowerCappingSwitch === 2" :placeholder="$t('configuration.powerCappingConfig.powerLimitPlaceholder')" />
          </div>
        </div>
        <div class="labelBox">
          <div class="labelLeft2">{{ $t('configuration.powerCappingConfig.powerConsumLimit') }}</div>
          <div class="labelRight">
            <el-input v-model="formData.PowerLimitRedundancy" type="number" autocomplete="off" :disabled="formData.PowerCappingSwitch === 2" :placeholder="$t('configuration.powerCappingConfig.powerConsumLimitPlaceholder')" />
          </div>
        </div>
        <div class="labelBox">
          <div class="labelLeft2">{{ $t('configuration.powerCappingConfig.currentTotalPower') }}</div>
          <div class="labelRight2">
            <div class="labelRightContent">{{ formData.CurrentTotalPower }}</div>
          </div>
        </div>
        <div class="labelBox">
          <div class="labelLeft">{{ $t('configuration.powerCappingConfig.powerLimitState') }}</div>
          <div class="labelRight2">
            <div class="labelRightContent">{{ formData.LimitStatus }}</div>
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
    var validatePowerLimit = (rule, value, callback) => {
      if (value === '') {
        callback(new Error(this.$t('configuration.powerCappingConfig.powerLimitPlaceholder')))
      } else {
        callback()
      }
    }
    var validatePowerLimitRedundancy = (rule, value, callback) => {
      if (value === '') {
        callback(new Error(this.$t('configuration.powerCappingConfig.powerConsumLimitPlaceholder')))
      } else {
        callback()
      }
    }
    return {
      formData: {
        'PowerCappingSwitch': 1,
        'PowerLimit': 280,
        'PowerLimitRedundancy': 123,
        'CurrentTotalPower': '1234',
        'LimitStatus': '4567'
      },
      formRules: {
        PowerLimit: [
          { validator: validatePowerLimit, trigger: 'change' }
        ],
        PowerLimitRedundancy: [
          { validator: validatePowerLimitRedundancy, trigger: 'change' }
        ]
      }
    }
  },
  methods: {
    submitForm() {
      this.$refs.ruleForm.validate(val => {
        if (!val) return
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
  margin: 12px 0;
  font-size: 14px;
  font-weight: 600;
  color: #666;
  .labelLeft {
    display: inline-block;
    width: 42%;
    text-align: right;
    padding: 10px 35px 10px 20px;
  }
  .labelLeft2 {
    display: inline-block;
    width: 42%;
    text-align: right;
    padding: 10px 30px 10px 20px;
  }
  .labelRight {
    display: inline-block;
    width: 58%;
    max-width: 250px;
    padding: 10px 0;
  }
  .labelRight2 {
    display: inline-block;
    width: 58%;
    max-width: 250px;
    padding: 10px 0;
  }
  .labelRightContent {
    font-weight: 600;
    color: #888;
  }
}
.el-switch {
  display: inline-block !important;
  margin-left: 30px;
}
.submitForm {
  padding: 0 15px 30px 15px;
  background-color: #fff;
  flex: 1;
  text-align: center;
  padding-left: 6.5%;
}
::v-deep {
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
  .el-switch__label {
    font-weight: 600;
    color: #666;
    width: 40px;
  }
  .el-switch__label.is-active {
    color: #1890ff;
  }
  .el-switch__label--left {
    margin-right: 0;
  }
  .el-switch__core {
    margin: 0 25px;
  }
  .el-switch__label--right {
    margin-left: 0;
  }
}
</style>

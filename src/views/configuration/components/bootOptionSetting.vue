<template>
  <div class="app-container">
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('configuration.bootOptionSettingConfig.bootOptionSettingConfigTitle') }}</span>
        </div>
        <div class="labelBox">
          <div class="labelLeft">{{ $t('configuration.bootOptionSettingConfig.bootType') }}</div>
          <div class="labelRight">
            <div>
              <el-radio v-model="formData.BootType" :label="0">Legacy</el-radio>
            </div>
            <div>
              <el-radio v-model="formData.BootType" :label="1">UEFI</el-radio>
            </div>
          </div>
        </div>
        <div class="labelBox">
          <div class="labelLeft">{{ $t('configuration.bootOptionSettingConfig.timeliness') }}</div>
          <div class="labelRight">
            <div>
              <el-radio v-model="formData.BootValid" :label="0">
                {{ $t('configuration.bootOptionSettingConfig.nextStartup') }}
              </el-radio>
            </div>
            <div>
              <el-radio v-model="formData.BootValid" :label="1">
                {{ $t('configuration.bootOptionSettingConfig.allStartup') }}
              </el-radio>
            </div>
          </div>
        </div>
        <div class="labelBox">
          <div class="labelLeft2">{{ $t('configuration.bootOptionSettingConfig.bootOption') }}</div>
          <div class="labelRight">
            <div>
              <el-radio v-model="formData.BootOption" :label="0">
                {{ $t('configuration.bootOptionSettingConfig.none') }}
              </el-radio>
            </div>
            <div>
              <el-radio v-model="formData.BootOption" :label="1">
                {{ $t('configuration.bootOptionSettingConfig.pxe') }}
              </el-radio>
            </div>
            <div>
              <el-radio v-model="formData.BootOption" :label="2">
                {{ $t('configuration.bootOptionSettingConfig.hardDrive') }}
              </el-radio>
            </div>
            <div>
              <el-radio v-model="formData.BootOption" :label="3">
                {{ $t('configuration.bootOptionSettingConfig.cd') }}
              </el-radio>
            </div>
            <div>
              <el-radio v-model="formData.BootOption" :label="4">
                {{ $t('configuration.bootOptionSettingConfig.bios') }}
              </el-radio>
            </div>
            <div>
              <el-radio v-model="formData.BootOption" :label="5">
                {{ $t('configuration.bootOptionSettingConfig.media') }}
              </el-radio>
            </div>
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
    return {
      formData: {
        BootType: 1,
        BootValid: 0,
        BootOption: 3
      }
    }
  },
  computed: {
  },
  methods: {
    submitForm() {
      console.log(this.formData)
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
  margin: 8px 0 0 0;
  font-size: 14px;
  // font-weight: 600;
  color: #666;
  .labelLeft {
    display: inline-block;
    width: 42%;
    text-align: right;
    padding: 10px 35px 10px 20px;
    font-weight: 600;
    height: 72.75px;
    vertical-align: middle;
  }
  .labelLeft2 {
    display: inline-block;
    width: 42%;
    text-align: right;
    padding: 10px 35px 10px 20px;
    font-weight: 600;
    height: 218.25px;
    vertical-align: middle;
  }
  .labelRight {
    display: inline-block;
    width: 58%;
    max-width: 250px;
    padding: 10px 0;
    font-weight: 600;
  }
}
.submitForm {
  margin-top: -70px;
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
  .el-input--suffix .el-input__inner {
    padding-right: 75px;
  }
  .el-radio {
    margin: 10px 0;
  }
}
</style>

<template>
  <div class="app-container">
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('configuration.timeSettingConfig.timeSettingConfigTitle') }}</span>
        </div>
        <div>
          <div class="labelBox">
            <div class="labelLeft">{{ $t('configuration.timeSettingConfig.currentTime') }}</div>
            <div class="labelRight">{{ currentTime }}</div>
          </div>
          <div class="labelBox">
            <div class="labelLeft">{{ $t('configuration.timeSettingConfig.timeZone') }}</div>
            <div class="labelRight">
              <el-select v-model="list.UtcOffset" popper-class="select-popper" :placeholder="$t('configuration.timeSettingConfig.timeZonePlaceholder')">
                <el-option v-for="item in utcOffsetList" :key="item.value" :label="item.name" :value="item.value" />
              </el-select>
            </div>
          </div>
          <div class="labelBox">
            <div class="labelLeft">{{ $t('configuration.timeSettingConfig.timeSettingConfigTitle') }}</div>
            <div class="labelRight">
              <el-date-picker
                v-model="list.TimeStamp"
                type="datetime"
                :clearable="false"
                :placeholder="$t('configuration.timeSettingConfig.timePlaceholder')"
                format="yyyy/MM/dd HH:mm:ss"
                :default-value="this.$moment(list.TimeStamp).format('YYYY/MM/DD')"
                :default-time="this.$moment(list.TimeStamp).format('HH:mm:ss')"
              />
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
      list: {
        TimeStamp: 0,
        UtcOffset: -480
      },
      originTimeStamp: '',
      utcOffsetList: [
        { name: 'GMT -12:00', value: 720 },
        { name: 'GMT -11:00', value: 660 },
        { name: 'GMT -10:00', value: 600 },
        { name: 'GMT -09:00', value: 540 },
        { name: 'GMT -08:00', value: 480 },
        { name: 'GMT -07:00', value: 420 },
        { name: 'GMT -06:00', value: 360 },
        { name: 'GMT -05:00', value: 300 },
        { name: 'GMT -04:00', value: 240 },
        { name: 'GMT -03:00', value: 180 },
        { name: 'GMT -02:00', value: 120 },
        { name: 'GMT -01:00', value: 60 },
        { value: 0, name: 'GMT +00:00' },
        { value: -60, name: 'GMT +01:00' },
        { value: -120, name: 'GMT +02:00' },
        { value: -180, name: 'GMT +03:00' },
        { value: -240, name: 'GMT +04:00' },
        { value: -300, name: 'GMT +05:00' },
        { value: -360, name: 'GMT +06:00' },
        { value: -420, name: 'GMT +07:00' },
        { value: -480, name: 'GMT +08:00' },
        { value: -540, name: 'GMT +09:00' },
        { value: -600, name: 'GMT +10:00' },
        { value: -660, name: 'GMT +11:00' },
        { value: -720, name: 'GMT +12:00' },
        { value: -780, name: 'GMT +13:00' },
        { value: -840, name: 'GMT +14:00' }
      ]
    }
  },
  computed: {
    currentTime: function() {
      return `${this.$moment(this.originTimeStamp).format('YYYY/MM/DD HH:mm:ss')} GMT ${this.$moment(this.originTimeStamp).format('Z')}`
    }
  },
  mounted() {
    this.getDate()
  },
  methods: {
    getDate() {
      this.originTimeStamp = this.list.TimeStamp
    },
    submitForm() {
      this.$confirm(this.$t('configuration.timeSettingConfig.comfirmMsg'), this.$t('configuration.userManageConfig.comfirmTitle'), {
        confirmButtonText: this.$t('configuration.userManageConfig.confirmButtonText'),
        cancelButtonText: this.$t('configuration.userManageConfig.cancelButtonText'),
        type: 'warning'
      }).then(() => {
        this.$message({
          type: 'success',
          duration: '1000',
          message: this.$t('configuration.timeSettingConfig.successMsg')
        })
        this.list.TimeStamp = this.$moment(this.list.TimeStamp).valueOf()
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
  font-weight: 600;
  color: #666;
  .labelLeft {
    display: inline-block;
    width: 42%;
    text-align: right;
    padding: 10px 35px 10px 20px;
  }
  .labelRight {
    display: inline-block;
    width: 58%;
    max-width: 250px;
    padding: 10px 0;
  }
}
.submitForm {
  padding: 15px 15px 30px 15px;
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
  // .select-popper {
  //   max-width: 250px !important;
  //   min-width: 250px !important;
  // }
}
</style>

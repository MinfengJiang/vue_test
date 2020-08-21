<template>
  <div class="app-container">
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('configuration.trapSettingConfig.trapSettingConfigTitle') }}</span>
        </div>
        <el-form ref="ruleForm" :model="formData" :rules="formRules" :label-width="`${formLabelWidth}px`">
          <el-form-item key="AlertLevel" :label="$t('configuration.trapSettingConfig.alertLevel')">
            <el-select v-model="formData.AlertLevel">
              <el-option :label="$t('configuration.trapSettingConfig.warningCritical')" :value="1" />
              <el-option :label="$t('configuration.trapSettingConfig.critical')" :value="2" />
              <el-option :label="$t('configuration.trapSettingConfig.all')" :value="3" />
              <el-option v-show="false" :label="$t('configuration.trapSettingConfig.unspecified')" :value="255" />
            </el-select>
          </el-form-item>
          <el-form-item key="DestinationType" :label="$t('configuration.trapSettingConfig.destinationType')">
            <el-select v-model="formData.DestinationType">
              <el-option label="SMTP" :value="6" />
              <el-option label="SNMP" :value="7" />
              <el-option v-show="false" :label="$t('configuration.trapSettingConfig.unspecified')" :value="255" />
            </el-select>
          </el-form-item>
          <el-form-item v-if="formData.DestinationType === 6" key="SmtpReceiverMailAddr" :label="$t('configuration.trapSettingConfig.smtpReceiverMailAddr')" prop="SmtpReceiverMailAddr">
            <el-input v-model="formData.SmtpReceiverMailAddr" autocomplete="off" :placeholder="$t('configuration.trapSettingConfig.smtpReceiverMailAddrPlaceholder')" />
          </el-form-item>
          <el-form-item v-if="formData.DestinationType === 6" key="SmtpEmailSubject" :label="$t('configuration.trapSettingConfig.smtpEmailSubject')">
            <el-input v-model="formData.SmtpEmailSubject" autocomplete="off" :placeholder="$t('configuration.trapSettingConfig.smtpEmailSubjectPlaceholder')" />
          </el-form-item>
          <el-form-item v-if="formData.DestinationType === 6" key="SmtpEmailContent" :label="$t('configuration.trapSettingConfig.smtpEmailContent')">
            <el-input v-model="formData.SmtpEmailContent" autocomplete="off" :placeholder="$t('configuration.trapSettingConfig.smtpEmailContentPlaceholder')" />
          </el-form-item>
          <el-form-item v-if="formData.DestinationType === 7" key="SnmpDestAddr" :label="$t('configuration.trapSettingConfig.snmpDestAddr')">
            <el-input v-model="formData.SnmpDestAddr" autocomplete="off" :placeholder="$t('configuration.trapSettingConfig.snmpDestAddrPlaceholder')" />
          </el-form-item>
          <el-form-item v-if="formData.DestinationType === 7" key="SnmptrapVersion" :label="$t('configuration.trapSettingConfig.snmpVersion')">
            <el-select v-model="formData.SnmptrapVersion">
              <el-option label="SNMPv1" :value="0" />
              <el-option label="SNMPv2c" :value="1" />
              <el-option label="SNMPv3" :value="2" />
            </el-select>
          </el-form-item>
          <el-form-item v-if="formData.SnmptrapVersion === 2" key="SnmptrapEngineID" :label="$t('configuration.trapSettingConfig.snmpEngineID')" prop="SnmptrapEngineID">
            <el-input v-model="formData.SnmptrapEngineID" type="number" autocomplete="off" :placeholder="$t('configuration.trapSettingConfig.snmpEngineIDPlaceholder')" />
          </el-form-item>
          <el-form-item v-if="formData.SnmptrapVersion === 2" key="SnmptrapUsername" :label="$t('configuration.trapSettingConfig.userName')" prop="SnmptrapUsername">
            <el-input v-model="formData.SnmptrapUsername" autocomplete="off" :placeholder="$t('configuration.trapSettingConfig.userNamePlaceholder')" />
          </el-form-item>
          <el-form-item v-if="formData.SnmptrapVersion === 2" key="SnmptrapAuthMethod" :label="$t('configuration.trapSettingConfig.authMethod')">
            <el-select v-model="formData.SnmptrapAuthMethod">
              <el-option label="NONE" :value="0" />
              <el-option label="MD5" :value="1" />
              <el-option label="SHA" :value="2" />
            </el-select>
          </el-form-item>
          <el-form-item v-if="authMethodShown" key="SnmptrapAuthPassword" :label="$t('configuration.trapSettingConfig.authPassword')" prop="SnmptrapAuthPassword">
            <el-input v-model="formData.SnmptrapAuthPassword" type="password" autocomplete="off" :placeholder="$t('configuration.trapSettingConfig.authPasswordPlaceholder')" />
          </el-form-item>
          <el-form-item v-if="authMethodShown" key="confirmAuthPassword" :label="$t('configuration.trapSettingConfig.confirmAuthPassword')" prop="confirmAuthPassword">
            <el-input v-model="formData.confirmAuthPassword" type="password" autocomplete="off" :placeholder="$t('configuration.trapSettingConfig.confirmAuthPasswordPlaceholder')" />
          </el-form-item>
          <el-form-item v-if="formData.SnmptrapVersion === 2" key="SnmptrapEncryptMethod" :label="$t('configuration.trapSettingConfig.encryptMethod')">
            <el-select v-model="formData.SnmptrapEncryptMethod">
              <el-option label="NONE" :value="0" />
              <el-option label="AES" :value="1" />
              <el-option label="DES" :value="2" />
            </el-select>
          </el-form-item>
          <el-form-item v-if="encryptMethodShown" key="SnmptrapEncryptPassword" :label="$t('configuration.trapSettingConfig.encryptPassword')" prop="SnmptrapEncryptPassword">
            <el-input v-model="formData.SnmptrapEncryptPassword" type="password" autocomplete="off" :placeholder="$t('configuration.trapSettingConfig.encryptPasswordPlaceholder')" />
          </el-form-item>
          <el-form-item v-if="encryptMethodShown" key="confirmEncryptPassword" :label="$t('configuration.trapSettingConfig.confirmEncryptPassword')" prop="confirmEncryptPassword">
            <el-input v-model="formData.confirmEncryptPassword" type="password" autocomplete="off" :placeholder="$t('configuration.trapSettingConfig.confirmEncryptPasswordPlaceholder')" />
          </el-form-item>
          <el-form-item class="submitBoxStyle">
            <el-button type="primary" style="margin-left: 50px" :disabled="formData.AlertLevel === 255 || formData.DestinationType === 255" @click="submitForm">{{ $t('configuration.userManageConfig.confirmBtn') }}</el-button>
          </el-form-item>
        </el-form>
      </div>
    </el-row>
  </div>
</template>

<script>
import { validEmail } from '@/utils/validate'

export default {
  data() {
    var validateSmtpReceiverMailAddr = (rule, value, callback) => {
      if (value === '') {
        callback(new Error(this.$t('configuration.trapSettingConfig.smtpReceiverMailAddrPlaceholder')))
      } else if (!validEmail(value)) {
        callback(new Error(this.$t('configuration.trapSettingConfig.smtpReceiverMailAddrErr')))
      } else {
        callback()
      }
    }
    var validateSnmptrapEngineID = (rule, value, callback) => {
      if (value === '') {
        callback(new Error(this.$t('configuration.trapSettingConfig.snmpEngineIDPlaceholder')))
      } else {
        callback()
      }
    }
    var validateSnmpUserName = (rule, value, callback) => {
      if (value === '') {
        callback(new Error(this.$t('configuration.trapSettingConfig.userNamePlaceholder')))
      } else {
        callback()
      }
    }
    var validateSnmpAuthPassword = (rule, value, callback) => {
      if (value === '') {
        callback(new Error(this.$t('configuration.trapSettingConfig.authPasswordPlaceholder')))
      } else {
        if (this.formData.confirmAuthPassword !== undefined && this.formData.confirmAuthPassword !== '') {
          this.$refs.ruleForm.validateField('confirmAuthPassword')
        }
        callback()
      }
    }
    var validateConfirmAuthPassword = (rule, value, callback) => {
      if (value === '') {
        callback(new Error(this.$t('configuration.trapSettingConfig.validatePassAgain')))
      } else if (value !== '' && value !== this.formData.SnmptrapAuthPassword) {
        callback(new Error(this.$t('configuration.trapSettingConfig.validatePassInconsistent')))
      } else {
        callback()
      }
    }
    var validateSnmpEncryptPassword = (rule, value, callback) => {
      if (value === '') {
        callback(new Error(this.$t('configuration.trapSettingConfig.encryptPasswordPlaceholder')))
      } else {
        if (this.formData.confirmEncryptPassword !== undefined && this.formData.confirmEncryptPassword !== '') {
          this.$refs.ruleForm.validateField('confirmEncryptPassword')
        }
        callback()
      }
    }
    var validateConfirmEncryptPassword = (rule, value, callback) => {
      if (value === '') {
        callback(new Error(this.$t('configuration.trapSettingConfig.validatePassAgain')))
      } else if (value !== '' && value !== this.formData.SnmptrapEncryptPassword) {
        callback(new Error(this.$t('configuration.trapSettingConfig.validatePassInconsistent')))
      } else {
        callback()
      }
    }
    return {
      formLabelWidth: this.$store.getters.sidebar.opened ? document.documentElement.clientWidth * 0.315 : document.documentElement.clientWidth * 0.315 + 65,
      formData: {
        'AlertLevel': 255,
        'DestinationType': 255,
        'SnmpDestAddr': '255',
        'SnmptrapVersion': 1,
        'SnmptrapUsername': 'traptest',
        'SnmptrapEngineID': '8001020304050607',
        'SnmptrapAuthMethod': 2,
        'SnmptrapAuthPassword': '11111111',
        'SnmptrapEncryptMethod': 0,
        'SnmptrapEncryptPassword': '',
        'SmtpReceiverMailAddr': '255',
        'SmtpEmailSubject': '255',
        'SmtpEmailContent': '255'
      },
      formRules: {
        SmtpReceiverMailAddr: [
          { required: true, validator: validateSmtpReceiverMailAddr, trigger: 'change' }
        ],
        SnmptrapEngineID: [
          { required: true, validator: validateSnmptrapEngineID, trigger: 'change' }
        ],
        SnmptrapUsername: [
          { required: true, validator: validateSnmpUserName, trigger: 'change' }
        ],
        SnmptrapAuthPassword: [
          { required: true, validator: validateSnmpAuthPassword, trigger: 'change' },
          { min: 8, max: 16, message: this.$t('configuration.trapSettingConfig.validatePassLenght'), trigger: 'blur' }
        ],
        confirmAuthPassword: [
          { required: true, validator: validateConfirmAuthPassword, trigger: 'change' }
        ],
        SnmptrapEncryptPassword: [
          { required: true, validator: validateSnmpEncryptPassword, trigger: 'change' },
          { min: 8, max: 16, message: this.$t('configuration.trapSettingConfig.validatePassLenght'), trigger: 'blur' }
        ],
        confirmEncryptPassword: [
          { required: true, validator: validateConfirmEncryptPassword, trigger: 'change' }
        ]
      }
    }
  },
  computed: {
    opened: function() {
      return this.$store.getters.sidebar.opened
    },
    authMethodShown: function() {
      return this.formData.SnmptrapVersion === 2 ? (this.formData.SnmptrapAuthMethod !== 0) : false
    },
    encryptMethodShown: function() {
      return this.formData.SnmptrapVersion === 2 ? (this.formData.SnmptrapEncryptMethod !== 0) : false
    },
    lang: function() {
      return this.$store.getters.language
    }
  },
  watch: {
    opened(newName, oldName) {
      if (newName) {
        this.formLabelWidth = document.documentElement.clientWidth * 0.315
      } else {
        this.formLabelWidth = document.documentElement.clientWidth * 0.315 + 65
      }
    },
    lang(newName, oldName) {
      this.unspecifiedHandle()
    }
  },
  mounted() {
    window.addEventListener('resize', () => {
      return (() => {
        if (this.opened) {
          this.formLabelWidth = document.documentElement.clientWidth * 0.315
        } else {
          this.formLabelWidth = document.documentElement.clientWidth * 0.315 + 65
        }
      })()
    }, false)
    this.unspecifiedHandle()
  },
  methods: {
    unspecifiedHandle() {
      if (this.formData.SmtpReceiverMailAddr === '255') {
        this.formData.SmtpReceiverMailAddr = this.$t('configuration.trapSettingConfig.unspecified')
      } else if (this.formData.SmtpReceiverMailAddr === '不明') {
        this.formData.SmtpReceiverMailAddr = this.$t('configuration.trapSettingConfig.unspecified')
      } else if (this.formData.SmtpReceiverMailAddr === 'Unspecified') {
        this.formData.SmtpReceiverMailAddr = this.$t('configuration.trapSettingConfig.unspecified')
      }
      if (this.formData.SmtpEmailSubject === '255') {
        this.formData.SmtpEmailSubject = this.$t('configuration.trapSettingConfig.unspecified')
      } else if (this.formData.SmtpEmailSubject === '不明') {
        this.formData.SmtpEmailSubject = this.$t('configuration.trapSettingConfig.unspecified')
      } else if (this.formData.SmtpEmailSubject === 'Unspecified') {
        this.formData.SmtpEmailSubject = this.$t('configuration.trapSettingConfig.unspecified')
      }
      if (this.formData.SmtpEmailContent === '255') {
        this.formData.SmtpEmailContent = this.$t('configuration.trapSettingConfig.unspecified')
      } else if (this.formData.SmtpEmailContent === '不明') {
        this.formData.SmtpEmailContent = this.$t('configuration.trapSettingConfig.unspecified')
      } else if (this.formData.SmtpEmailContent === 'Unspecified') {
        this.formData.SmtpEmailContent = this.$t('configuration.trapSettingConfig.unspecified')
      }
      if (this.formData.SnmpDestAddr === '255') {
        this.formData.SnmpDestAddr = this.$t('configuration.trapSettingConfig.unspecified')
      } else if (this.formData.SnmpDestAddr === '不明') {
        this.formData.SnmpDestAddr = this.$t('configuration.trapSettingConfig.unspecified')
      } else if (this.formData.SnmpDestAddr === 'Unspecified') {
        this.formData.SnmpDestAddr = this.$t('configuration.trapSettingConfig.unspecified')
      }
    },
    submitForm() {
      this.$refs.ruleForm.validate(val => {
        if (!val) return
        if (this.formData.confirmAuthPassword !== undefined) {
          delete this.formData['confirmAuthPassword']
        }
        if (this.formData.confirmEncryptPassword !== undefined) {
          delete this.formData['confirmEncryptPassword']
        }
        // console.log(this.formData)
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
.submitBoxStyle {
  margin: 40px 0 25px 35px;
}
::v-deep {
  .el-form {
    margin-top: 20px;
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
  .el-date-editor.el-input, .el-date-editor.el-input__inner {
    width: 100%;
  }
  .el-input--suffix .el-input__inner {
    padding-right: 75px;
  }
}
.userIdStyle {
  // font-size: 16px;
  font-weight: 700;
}
</style>

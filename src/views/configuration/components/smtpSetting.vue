<template>
  <div class="app-container">
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('configuration.smtpSettingConfig.smtpSettingConfigTitle') }}</span>
        </div>
        <el-form ref="ruleForm" :model="formData" :rules="formRules" :label-width="`${formLabelWidth}px`">
          <el-form-item :label="$t('configuration.smtpSettingConfig.lanChannel')">
            <el-select v-model="formData.LanChannelNum" :placeholder="$t('configuration.smtpSettingConfig.lanChannelPlaceholder')" @change="channelHandle">
              <el-option label="1" :value="1" />
              <el-option label="2" :value="2" />
              <el-option label="3" :value="3" />
            </el-select>
          </el-form-item>
          <el-form-item :label="$t('configuration.smtpSettingConfig.smtpServer')" prop="SmtpServerIP">
            <el-input v-model="formData.SmtpServerIP" autocomplete="off" :placeholder="$t('configuration.smtpSettingConfig.smtpServerPlaceholder')" />
          </el-form-item>
          <el-form-item :label="$t('configuration.smtpSettingConfig.port')">
            <el-input v-model="formData.SmtpServerPort" autocomplete="off" placeholder="25" />
          </el-form-item>
          <el-form-item :label="$t('configuration.smtpSettingConfig.senderEmail')" prop="SenderEmailAddr">
            <el-input v-model="formData.SenderEmailAddr" autocomplete="off" :placeholder="$t('configuration.smtpSettingConfig.senderEmailPlaceholder')" />
          </el-form-item>
          <el-form-item :label="$t('configuration.smtpSettingConfig.authMethod')">
            <el-select v-model="formData.AuthMethod" :placeholder="$t('configuration.smtpSettingConfig.authMethodPlaceholder')">
              <el-option label="PLAIN" :value="0" />
              <!-- <el-option label="STARTTLS_OR_PLAIN" :value="1" disabled /> -->
              <el-option label="STARTTLS" :value="2" />
              <el-option label="SSL" :value="3" />
            </el-select>
          </el-form-item>
          <el-form-item :label="$t('configuration.smtpSettingConfig.userName')">
            <el-input v-model="formData.AuthUsername" autocomplete="off" :placeholder="$t('configuration.smtpSettingConfig.userNamePlaceholder')" />
          </el-form-item>
          <el-form-item :label="$t('configuration.smtpSettingConfig.password')">
            <el-input v-model="formData.AuthPasswd" autocomplete="off" :placeholder="$t('configuration.smtpSettingConfig.passwordPlaceholder')" />
          </el-form-item>
          <el-form-item :label="$t('configuration.smtpSettingConfig.enable')">
            <el-checkbox v-model="formData.SmtpEnable" :true-label="0" :false-label="1" />
          </el-form-item>
          <el-form-item class="submitBoxStyle">
            <el-button type="primary" style="margin-left: 50px" @click="submitForm">{{ $t('configuration.userManageConfig.confirmBtn') }}</el-button>
          </el-form-item>
        </el-form>
      </div>
    </el-row>
  </div>
</template>

<script>
import { validAlphabets, validEmail } from '@/utils/validate'

export default {
  data() {
    var validateServerIP = (rule, value, callback) => {
      if (value !== '' && !validAlphabets(value)) {
        callback(new Error(this.$t('configuration.ldapSettingConfig.validateServerIP')))
      } else {
        callback()
      }
    }
    var validateEmailAddr = (rule, value, callback) => {
      if (value !== '' && !validEmail(value)) {
        callback(new Error(this.$t('configuration.smtpSettingConfig.validateEmailAddr')))
      } else {
        callback()
      }
    }
    return {
      list: [
        {
          'LanChannelNum': 1,
          'SmtpServerIP': '',
          'SmtpServerPort': 0,
          'SenderEmailAddr': '',
          'AuthMethod': 0,
          'AuthUsername': '',
          'SmtpEnable': 0
        },
        {
          'LanChannelNum': 2,
          'SmtpServerIP': '192.168.10.3',
          'SmtpServerPort': 8080,
          'SenderEmailAddr': 'zhanglun@byosoft.com.cn',
          'AuthMethod': 1,
          'AuthUsername': 'lydia',
          'SmtpEnable': 1
        },
        {
          'LanChannelNum': 3,
          'SmtpServerIP': '',
          'SmtpServerPort': 0,
          'SenderEmailAddr': '',
          'AuthMethod': 2,
          'AuthUsername': '',
          'SmtpEnable': 0
        }
      ],
      formLabelWidth: this.$store.getters.sidebar.opened ? document.documentElement.clientWidth * 0.315 : document.documentElement.clientWidth * 0.315 + 65,
      formData: {
        'LanChannelNum': 1,
        'SmtpServerIP': '',
        'SmtpServerPort': 0,
        'SenderEmailAddr': '',
        'AuthMethod': 0,
        'AuthUsername': '',
        'SmtpEnable': 0
      },
      originPassword: '',
      formRules: {
        SmtpServerIP: [
          { validator: validateServerIP, trigger: 'change' }
        ],
        SenderEmailAddr: [
          { validator: validateEmailAddr, trigger: 'change' }
        ]
      }
    }
  },
  computed: {
    opened: function() {
      return this.$store.getters.sidebar.opened
    }
  },
  watch: {
    opened(newName, oldName) {
      if (newName) {
        this.formLabelWidth = document.documentElement.clientWidth * 0.315
      } else {
        this.formLabelWidth = document.documentElement.clientWidth * 0.315 + 65
      }
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

    this.getList()
  },
  methods: {
    getList() {
      this.formData = this.list[0]
    },
    channelHandle(val) {
      this.formData = {}
      this.formData = this.list[val - 1]
    },
    submitForm() {
      this.$refs.ruleForm.validate(val => {
        if (!val) return
        // is password changed
        if (this.formData.AuthPasswd === '' || this.formData.AuthPasswd === undefined) {
          this.formData.change_pwd = 1 // unchange
        } else {
          this.formData.change_pwd = 0 // changed
        }
        // console.log(this.formData)
        this.$refs.ruleForm.resetFields()
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

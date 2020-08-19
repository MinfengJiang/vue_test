<template>
  <div class="app-container">
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('configuration.snmpSettingConfig.snmpSettingConfigTitle') }}</span>
        </div>
        <el-form ref="ruleForm" :model="formData" :rules="formRules" :label-width="`${formLabelWidth}px`">
          <el-form-item :label="$t('configuration.snmpSettingConfig.enable')">
            <el-checkbox v-model="formData.SnmpEnable" :true-label="0" :false-label="1" />
          </el-form-item>
          <el-form-item :label="$t('configuration.snmpSettingConfig.snmpVersion')">
            <el-select v-model="formData.SnmpVersion" :disabled="formData.SnmpEnable !== 0">
              <el-option label="SNMPv1" :value="0" />
              <el-option label="SNMPv2c" :value="1" />
              <el-option label="SNMPv3" :value="2" />
            </el-select>
          </el-form-item>
          <el-form-item v-if="formData.SnmpVersion !== 2" key="SnmpRoCommunity" :label="$t('configuration.snmpSettingConfig.RoCommunity')">
            <el-input v-model="formData.SnmpRoCommunity" :disabled="formData.SnmpEnable !== 0" autocomplete="off" :placeholder="$t('configuration.snmpSettingConfig.RoCommunityPlaceholder')" />
          </el-form-item>
          <el-form-item v-if="formData.SnmpVersion !== 2" key="SnmpRwCommunity" :label="$t('configuration.snmpSettingConfig.RwCommunity')">
            <el-input v-model="formData.SnmpRwCommunity" :disabled="formData.SnmpEnable !== 0" autocomplete="off" :placeholder="$t('configuration.snmpSettingConfig.RwCommunityPlaceholder')" />
          </el-form-item>
          <el-form-item v-if="formData.SnmpVersion === 2" key="SnmpUserAccessMode" :label="$t('configuration.snmpSettingConfig.userAccessMode')">
            <el-select v-model="formData.SnmpUserAccessMode" :disabled="formData.SnmpEnable !== 0">
              <el-option label="Read-Only" :value="0" />
              <el-option label="Read-Write" :value="1" />
            </el-select>
          </el-form-item>
          <el-form-item v-if="formData.SnmpVersion === 2" key="SnmpUsername" :label="$t('configuration.snmpSettingConfig.userName')" prop="SnmpUsername">
            <el-input v-model="formData.SnmpUsername" :disabled="formData.SnmpEnable !== 0" autocomplete="off" :placeholder="$t('configuration.snmpSettingConfig.userNamePlaceholder')" />
          </el-form-item>
          <el-form-item v-if="formData.SnmpVersion === 2" key="AuthMethod" :label="$t('configuration.snmpSettingConfig.authMethod')">
            <el-select v-model="formData.AuthMethod" :disabled="formData.SnmpEnable !== 0">
              <el-option label="NONE" :value="0" />
              <el-option label="MD5" :value="1" />
              <el-option label="SHA" :value="2" />
            </el-select>
          </el-form-item>
          <el-form-item v-if="authMethodShown" key="SnmpAuthPassword" :label="$t('configuration.snmpSettingConfig.authPassword')" prop="SnmpAuthPassword">
            <el-input v-model="formData.SnmpAuthPassword" :disabled="formData.SnmpEnable !== 0" type="password" autocomplete="off" :placeholder="$t('configuration.snmpSettingConfig.authPasswordPlaceholder')" />
          </el-form-item>
          <el-form-item v-if="authMethodShown" key="confirmAuthPassword" :label="$t('configuration.snmpSettingConfig.confirmAuthPassword')" prop="confirmAuthPassword">
            <el-input v-model="formData.confirmAuthPassword" :disabled="formData.SnmpEnable !== 0" type="password" autocomplete="off" :placeholder="$t('configuration.snmpSettingConfig.confirmAuthPasswordPlaceholder')" />
          </el-form-item>
          <el-form-item v-if="formData.SnmpVersion === 2" key="EncryptMethod" :label="$t('configuration.snmpSettingConfig.encryptMethod')">
            <el-select v-model="formData.EncryptMethod" :disabled="formData.SnmpEnable !== 0">
              <el-option label="NONE" :value="0" />
              <el-option label="AES" :value="1" />
              <el-option label="DES" :value="2" />
            </el-select>
          </el-form-item>
          <el-form-item v-if="encryptMethodShown" key="SnmpEncryptPassword" :label="$t('configuration.snmpSettingConfig.encryptPassword')" prop="SnmpEncryptPassword">
            <el-input v-model="formData.SnmpEncryptPassword" :disabled="formData.SnmpEnable !== 0" type="password" autocomplete="off" :placeholder="$t('configuration.snmpSettingConfig.encryptPasswordPlaceholder')" />
          </el-form-item>
          <el-form-item v-if="encryptMethodShown" key="confirmEncryptPassword" :label="$t('configuration.snmpSettingConfig.confirmEncryptPassword')" prop="confirmEncryptPassword">
            <el-input v-model="formData.confirmEncryptPassword" :disabled="formData.SnmpEnable !== 0" type="password" autocomplete="off" :placeholder="$t('configuration.snmpSettingConfig.confirmEncryptPasswordPlaceholder')" />
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
export default {
  data() {
    var validateSnmpUserName = (rule, value, callback) => {
      if (value === '') {
        callback(new Error(this.$t('configuration.snmpSettingConfig.userNamePlaceholder')))
      } else {
        callback()
      }
    }
    var validateSnmpAuthPassword = (rule, value, callback) => {
      if (value === '') {
        callback(new Error(this.$t('configuration.snmpSettingConfig.authPasswordPlaceholder')))
      } else {
        if (this.formData.confirmAuthPassword !== undefined && this.formData.confirmAuthPassword !== '') {
          this.$refs.ruleForm.validateField('confirmAuthPassword')
        }
        callback()
      }
    }
    var validateConfirmAuthPassword = (rule, value, callback) => {
      if (value === '') {
        callback(new Error(this.$t('configuration.snmpSettingConfig.validatePassAgain')))
      } else if (value !== '' && value !== this.formData.SnmpAuthPassword) {
        callback(new Error(this.$t('configuration.snmpSettingConfig.validatePassInconsistent')))
      } else {
        callback()
      }
    }
    var validateSnmpEncryptPassword = (rule, value, callback) => {
      if (value === '') {
        callback(new Error(this.$t('configuration.snmpSettingConfig.encryptPasswordPlaceholder')))
      } else {
        if (this.formData.confirmEncryptPassword !== undefined && this.formData.confirmEncryptPassword !== '') {
          this.$refs.ruleForm.validateField('confirmEncryptPassword')
        }
        callback()
      }
    }
    var validateConfirmEncryptPassword = (rule, value, callback) => {
      if (value === '') {
        callback(new Error(this.$t('configuration.snmpSettingConfig.validatePassAgain')))
      } else if (value !== '' && value !== this.formData.SnmpEncryptPassword) {
        callback(new Error(this.$t('configuration.snmpSettingConfig.validatePassInconsistent')))
      } else {
        callback()
      }
    }
    return {
      formLabelWidth: this.$store.getters.sidebar.opened ? document.documentElement.clientWidth * 0.315 : document.documentElement.clientWidth * 0.315 + 65,
      formData: {
        'SnmpEnable': 1,
        'SnmpVersion': 1,
        'AuthMethod': 0,
        'EncryptMethod': 0,
        'SnmpUserAccessMode': 0,
        'SnmpUsername': '',
        'SnmpAuthPassword': '',
        'SnmpEncryptPassword': '',
        'SnmpRoCommunity': 'public',
        'SnmpRwCommunity': 'private'
      },
      formRules: {
        SnmpUsername: [
          { required: true, validator: validateSnmpUserName, trigger: 'change' }
        ],
        SnmpAuthPassword: [
          { required: true, validator: validateSnmpAuthPassword, trigger: 'change' },
          { min: 8, max: 16, message: this.$t('configuration.snmpSettingConfig.validatePassLenght'), trigger: 'blur' }
        ],
        confirmAuthPassword: [
          { required: true, validator: validateConfirmAuthPassword, trigger: 'change' }
        ],
        SnmpEncryptPassword: [
          { required: true, validator: validateSnmpEncryptPassword, trigger: 'change' },
          { min: 8, max: 16, message: this.$t('configuration.snmpSettingConfig.validatePassLenght'), trigger: 'blur' }
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
      return this.formData.SnmpVersion === 2 ? (this.formData.AuthMethod !== 0) : false
    },
    encryptMethodShown: function() {
      return this.formData.SnmpVersion === 2 ? (this.formData.EncryptMethod !== 0) : false
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
  },
  methods: {
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

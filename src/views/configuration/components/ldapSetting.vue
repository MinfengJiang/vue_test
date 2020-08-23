<template>
  <div class="app-container">
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <div :class="activeTab === 0 ? 'tabStyleActive' : 'tabStyle'" @click="tabActiveHandle(0)">
            <span>{{ $t('configuration.ldapSettingConfig.ldapSettingConfigTitle') }}</span>
          </div>
          <div :class="activeTab === 1 ? 'tabStyleActive' : 'tabStyle'" @click="tabActiveHandle(1)">
            <span>{{ $t('configuration.ldapSettingConfig.ldapSettingConfigTitle2') }}</span>
          </div>
        </div>
        <el-form ref="ruleForm" :model="formData" :rules="formRules" :label-width="`${formLabelWidth}px`">
          <el-form-item :label="$t('configuration.ldapSettingConfig.enable')">
            <el-checkbox v-if="activeTab === 0" key="ldap" v-model="formData.LDAPEnable" :true-label="0" :false-label="1" />
            <el-checkbox v-else key="ad" v-model="formData.ADEnable" :true-label="0" :false-label="1" />
          </el-form-item>
          <el-form-item :label="$t('configuration.ldapSettingConfig.server')" prop="ServerIP">
            <el-input v-model="formData.ServerIP" autocomplete="off" />
          </el-form-item>
          <el-form-item :label="$t('configuration.ldapSettingConfig.port')">
            <el-input v-model="formData.ServerPort" type="number" autocomplete="off" placeholder="25" />
          </el-form-item>
          <el-form-item :label="$t('configuration.ldapSettingConfig.baseDN')">
            <el-input v-model="formData.BaseDN" autocomplete="off" placeholder="dc=example,dc=com" />
          </el-form-item>
          <el-form-item :label="$t('configuration.ldapSettingConfig.adminBindDN')">
            <el-input v-model="formData.BindDN" autocomplete="off" placeholder="cn=Administrator,cn=User,dc=example,dc=com" />
          </el-form-item>
          <el-form-item :label="$t('configuration.ldapSettingConfig.adminBindPassword')">
            <el-input v-model="formData.BindPassword" type="password" autocomplete="off" placeholder="Password" />
          </el-form-item>
          <el-form-item :label="$t('configuration.ldapSettingConfig.userSearchDN')">
            <span slot="label">
              {{ $t('configuration.ldapSettingConfig.userSearchDN') }}
              <el-tooltip placement="top-start">
                <div slot="content">{{ $t('configuration.ldapSettingConfig.userSearchDNHelp') }}</div>
                <i class="el-icon-question" />
              </el-tooltip>
            </span>
            <el-input v-model="formData.UsrSearchDN" autocomplete="off" placeholder="cn=User,dc=example,dc=com" />
          </el-form-item>
          <el-form-item :label="$t('configuration.ldapSettingConfig.userSearchScope')">
            <el-input v-model="formData.UsrSearchScope" autocomplete="off" placeholder="sAMAccountName" />
          </el-form-item>
          <el-form-item :label="$t('configuration.ldapSettingConfig.userIDAttribute')">
            <el-input v-model="formData.UsrIDAttribute" autocomplete="off" placeholder="sAMAccountName" />
          </el-form-item>
          <el-form-item :label="$t('configuration.ldapSettingConfig.userPrivilege')">
            <el-select v-model="formData.UsrPrivi" :placeholder="$t('configuration.userManageConfig.placeholder')">
              <el-option :label="$t('configuration.userManageConfig.user')" :value="2" />
              <el-option :label="$t('configuration.userManageConfig.operator')" :value="3" />
              <el-option :label="$t('configuration.userManageConfig.administrator')" :value="4" />
            </el-select>
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
import { validAlphabets } from '@/utils/validate'

export default {
  data() {
    var validateServerIP = (rule, value, callback) => {
      if (value !== '' && !validAlphabets(value)) {
        callback(new Error(this.$t('configuration.ldapSettingConfig.validateServerIP')))
      } else {
        callback()
      }
    }
    return {
      formLabelWidth: this.$store.getters.sidebar.opened ? document.documentElement.clientWidth * 0.315 : document.documentElement.clientWidth * 0.315 + 65,
      formData: {},
      formData0: {
        'LDAPEnable': 0,
        'ServerIP': '123',
        'ServerPort': 20,
        'EnableTLS': 1,
        'BaseDN': 'a',
        'BindDN': 'b',
        'BindPassword': 'c',
        'UsrSearchDN': 'd',
        'UsrSearchScope': 0,
        'UsrIDAttribute': 'e',
        'UsrPrivi': 3
      },
      formData1: {
        'ADEnable': 1,
        'ServerIP': '',
        'ServerPort': 0,
        'EnableTLS': 1,
        'BaseDN': '',
        'BindDN': '',
        'BindPassword': '',
        'UsrSearchDN': '',
        'UsrSearchScope': 0,
        'UsrIDAttribute': '',
        'UsrPrivi': 4
      },
      activeTab: 0,
      formRules: {
        ServerIP: [
          { validator: validateServerIP, trigger: 'change' }
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
      this.formData = this.formData0
    },
    tabActiveHandle(val) {
      this.activeTab = val
      if (val === 0) {
        this.formData = this.formData0
      } else {
        this.formData = this.formData1
      }
    },
    submitForm() {
      this.$refs.ruleForm.validate(val => {
        if (!val) return
        console.log(this.formData)
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
  margin: -15px 0 0 -15px;
  span {
    /*padding-left: 12px;*/
    font-size: 16px;
    font-weight: 600;
  }
  .tabStyle {
    display: inline-block;
    width: 75px;
    height: 40px;
    line-height: 40px;
    text-align: center;
    cursor: pointer;
  }
  .tabStyleActive {
    display: inline-block;
    width: 75px;
    height: 40px;
    line-height: 40px;
    color: #1890ff;
    border-radius: 0 0 2px 0;
    border-top: 3px solid #1890ff;
    text-align: center;
    cursor: pointer;
    box-shadow: 0 0 2px 0 rgba(0,0,0,.08), 0 0 2px 0 rgba(0,0,0,.04);
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
</style>

<template>
  <div class="app-container">
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('configuration.userManageConfig.userManageConfigTitle') }}</span>
        </div>
        <el-table :data="list" style="width: 100%;padding-top: 10px;">
          <el-table-column :label="$t('configuration.userManageConfig.ID')" width="90px">
            <template slot-scope="scope">{{ scope.row.UserID }}</template>
          </el-table-column>
          <el-table-column :label="$t('configuration.userManageConfig.userName')" min-width="120px">
            <template v-if="scope.row.Status === 0" slot-scope="scope">{{ scope.row.Data.UserName }}</template>
            <template v-else />
          </el-table-column>
          <el-table-column :label="$t('configuration.userManageConfig.userPrivilege')" width="120px">
            <template v-if="scope.row.Status === 0" slot-scope="scope">
              <div v-if="scope.row.Data.UserPrivilege === 0" />
              <div v-else-if="scope.row.Data.UserPrivilege === 1">{{ $t('configuration.userManageConfig.callback') }}</div>
              <div v-else-if="scope.row.Data.UserPrivilege === 2">{{ $t('configuration.userManageConfig.user') }}</div>
              <div v-else-if="scope.row.Data.UserPrivilege === 3">{{ $t('configuration.userManageConfig.operator') }}</div>
              <div v-else-if="scope.row.Data.UserPrivilege === 4">{{ $t('configuration.userManageConfig.administrator') }}</div>
              <!-- <div v-else-if="scope.row.Data.UserPrivilege === 5">{{ $t('configuration.userManageConfig.OEM') }}</div> -->
              <div v-else-if="scope.row.Data.UserPrivilege === 15">{{ $t('configuration.userManageConfig.access') }}</div>
            </template>
            <template v-else />
          </el-table-column>
          <el-table-column :label="$t('configuration.userManageConfig.userEnable')" align="center" header-align="center" min-width="250px">
            <template slot-scope="scope">
              <div v-if="scope.row.Status === 0">
                <span style="color: #67c23a">{{ $t('configuration.userManageConfig.enable') }}</span>
              </div>
              <div v-else>
                <span style="color: #f56c6c">{{ $t('configuration.userManageConfig.disable') }}</span>
              </div>
            </template>
          </el-table-column>
          <el-table-column :label="$t('configuration.userManageConfig.action')" align="center" header-align="center" width="250px">
            <template slot-scope="scope">
              <div>
                <el-button type="primary" size="mini" @click="addUserHandle(scope.row, 'add')">{{ $t('configuration.userManageConfig.add') }}</el-button>
                <el-button type="warning" size="mini" @click="editUserHandle(scope.row, 'edit')">{{ $t('configuration.userManageConfig.modify') }}</el-button>
                <el-button type="danger" size="mini" @click="deleteUserHandle(scope.row)">{{ $t('configuration.userManageConfig.delete') }}</el-button>
              </div>
            </template>
          </el-table-column>
        </el-table>
      </div>
    </el-row>
    <el-dialog :title="$t('configuration.userManageConfig.addUserTitle')" :visible.sync="dialogVisible" :before-close="resetForm">
      <el-form ref="ruleForm" :model="form" :rules="formRules" :label-width="`${formLabelWidth}px`">
        <el-form-item :label="$t('configuration.userManageConfig.addUserTitle')">
          <div class="userIdStyle">{{ form.user_id }}</div>
        </el-form-item>
        <el-form-item prop="user_name">
          <span slot="label">
            {{ $t('configuration.userManageConfig.userName') }}
            <el-tooltip placement="top-start">
              <div slot="content">
                {{ $t('configuration.userManageConfig.userNameHelp') }}<br>
                {{ $t('configuration.userManageConfig.userNameHelp2') }}<br>
                {{ $t('configuration.userManageConfig.userNameHelp3') }}<br>
                {{ $t('configuration.userManageConfig.userNameHelp4') }}
              </div>
              <i class="el-icon-question" />
            </el-tooltip>
          </span>
          <el-input v-model="form.user_name" autocomplete="off" />
        </el-form-item>
        <el-form-item v-if="action !== 'add'" :label="$t('configuration.userManageConfig.password')">
          <el-checkbox v-model="form.change_pwd" :true-label="1" :false-label="0" @change="handleChecked" />
        </el-form-item>
        <el-form-item :label="$t('configuration.userManageConfig.passwordSize')">
          <el-radio v-model="form.password_size" :label="16">{{ $t('configuration.userManageConfig.passwordSize16') }}</el-radio>
          <el-radio v-model="form.password_size" :label="20">{{ $t('configuration.userManageConfig.passwordSize20') }}</el-radio>
        </el-form-item>
        <el-form-item v-if="action !== 'add'" :label="$t('configuration.userManageConfig.oldPassword')" :prop="propUserName">
          <el-input v-model="form.check_old_password" type="password" autocomplete="off" :disabled="action === 'edit' && form.change_pwd === 0" />
        </el-form-item>
        <el-form-item :prop="propPass">
          <span slot="label">
            {{ $t('configuration.userManageConfig.newPassword') }}
            <el-tooltip placement="top-start">
              <div slot="content">{{ $t('configuration.userManageConfig.newPasswordHelp') }}</div>
              <i class="el-icon-question" />
            </el-tooltip>
          </span>
          <el-input v-model="form.user_old_password" type="password" autocomplete="off" :disabled="action === 'edit' && form.change_pwd === 0" />
        </el-form-item>
        <el-form-item :label="$t('configuration.userManageConfig.confirmPass')" :prop="propCheckPass">
          <el-input v-model="form.user_password" type="password" autocomplete="off" :disabled="action === 'edit' && form.change_pwd === 0" />
        </el-form-item>
        <el-form-item :label="$t('configuration.userManageConfig.passExpiration')" prop="pwd_change_cycle">
          <el-input v-model.number="form.pwd_change_cycle" autocomplete="off" />
        </el-form-item>
        <el-form-item :label="$t('configuration.userManageConfig.userPrivilege')">
          <el-select v-model="form.usr_privilege" :placeholder="$t('configuration.userManageConfig.placeholder')">
            <el-option :label="$t('configuration.userManageConfig.callback')" :value="1" />
            <el-option :label="$t('configuration.userManageConfig.user')" :value="2" />
            <el-option :label="$t('configuration.userManageConfig.operator')" :value="3" />
            <el-option :label="$t('configuration.userManageConfig.administrator')" :value="4" />
            <!-- <el-option :label="$t('configuration.userManageConfig.OEM')" :value="5" /> -->
            <el-option :label="$t('configuration.userManageConfig.access')" :value="15" />
          </el-select>
        </el-form-item>
        <el-form-item :label="$t('configuration.userManageConfig.enableUser')">
          <el-checkbox v-model="form.user_enable" :true-label="1" :false-label="0" />
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button style="margin-left: -18px" @click="resetForm">{{ $t('configuration.userManageConfig.cancelBtn') }}</el-button>
        <el-button type="primary" style="margin-left: 50px" @click="submitForm">{{ $t('configuration.userManageConfig.confirmBtn') }}</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
export default {
  data() {
    var validateUserName = (rule, value, callback) => {
      if (value === '') {
        callback(new Error(this.$t('configuration.userManageConfig.validateUserName')))
      } else {
        // 至少一个字符。首字符必须是'a-z','A-Z','0-9','_'，其后字符必须是'a-z','A-Z','0-9', '_', '-', '.'。另外，'$'仅可以用作最后一个字符，不能用作开头。用户名不支持中文字符
        callback()
      }
    }
    var validateOldPass = (rule, value, callback) => {
      if (value === '') {
        callback(new Error(this.$t('configuration.userManageConfig.validatePass')))
      } else {
        callback()
      }
    }
    var validatePass = (rule, value, callback) => {
      if (value === '') {
        callback(new Error(this.$t('configuration.userManageConfig.validatePass')))
      } else if (this.form.check_old_password !== '' && value === this.form.check_old_password) {
        callback(new Error(this.$t('configuration.userManageConfig.validateOldPassInconsistent')))
      } else {
        if (this.form.user_password !== '') {
          this.$refs.ruleForm.validateField('user_password')
        }
        callback()
      }
    }
    var validateCheckPass = (rule, value, callback) => {
      if (value === '') {
        callback(new Error(this.$t('configuration.userManageConfig.validatePassAgain')))
      } else if (value !== this.form.user_old_password) {
        callback(new Error(this.$t('configuration.userManageConfig.validatePassInconsistent')))
      } else {
        callback()
      }
    }
    var validatePassExpiration = (rule, value, callback) => {
      if (value === '') {
        callback(new Error(this.$t('configuration.userManageConfig.validatePassExpiration')))
      } else {
        callback()
      }
    }
    return {
      list: [
        {
          'UserID': 1,
          'Status': 0,
          'Data': {
            'UserName': 'anonymous',
            'UserEnable': 0,
            'UserWebAccess': 0,
            'UserSSHAccess': 0,
            'UserKVMAccess': 0,
            'UserPrivilege': 0,
            'PwdChangeCycle': 30
          }
        },
        {
          'UserID': 2,
          'Status': 0,
          'Data': {
            'UserName': 'ADMIN',
            'UserEnable': 0,
            'UserWebAccess': 0,
            'UserSSHAccess': 0,
            'UserKVMAccess': 0,
            'UserPrivilege': 4,
            'PwdChangeCycle': 1000
          }
        },
        {
          'UserID': 3,
          'Status': 6009
        },
        {
          'UserID': 4,
          'Status': 6009
        },
        {
          'UserID': 5,
          'Status': 0,
          'Data': {
            'UserName': 'test',
            'UserEnable': 0,
            'UserWebAccess': 1,
            'UserSSHAccess': 1,
            'UserKVMAccess': 1,
            'UserPrivilege': 255,
            'PwdChangeCycle': 60
          }
        },
        {
          'UserID': 6,
          'Status': 6009
        },
        {
          'UserID': 7,
          'Status': 6009
        },
        {
          'UserID': 8,
          'Status': 0,
          'Data': {
            'UserName': 'zhanlun',
            'UserEnable': 0,
            'UserWebAccess': 0,
            'UserSSHAccess': 0,
            'UserKVMAccess': 0,
            'UserPrivilege': 1,
            'PwdChangeCycle': 11650
          }
        },
        {
          'UserID': 9,
          'Status': 6009
        },
        {
          'UserID': 10,
          'Status': 6009
        },
        {
          'UserID': 11,
          'Status': 6009
        },
        {
          'UserID': 12,
          'Status': 6009
        },
        {
          'UserID': 13,
          'Status': 6009
        },
        {
          'UserID': 14,
          'Status': 6009
        },
        {
          'UserID': 15,
          'Status': 6009
        }
      ],
      dialogVisible: false,
      formLabelWidth: document.documentElement.clientWidth * 0.205,
      form: {
        user_id: 1,
        user_name: '',
        change_pwd: 0,
        password_size: 16,
        check_old_password: '',
        user_old_password: '',
        user_password: '',
        user_enable: 0,
        pwd_change_cycle: 11650,
        usr_privilege: 1
      },
      action: '',
      formRules: {
        user_name: [
          { required: true, validator: validateUserName, trigger: 'change' }
        ],
        check_old_password: [
          { required: true, validator: validateOldPass, trigger: 'change' }
        ],
        user_old_password: [
          { required: true, validator: validatePass, trigger: 'change' }
        ],
        user_password: [
          { required: true, validator: validateCheckPass, trigger: 'change' }
        ],
        pwd_change_cycle: [
          { required: true, validator: validatePassExpiration, trigger: 'change' }
        ]
      }
    }
  },
  computed: {
    propUserName: function() {
      return ((this.action === 'edit' && this.form.change_pwd === 0) ? '!@#$%^&*' : 'check_old_password')
    },
    propPass: function() {
      return ((this.action === 'edit' && this.form.change_pwd === 0) ? '!@#$%^&*' : 'user_old_password')
    },
    propCheckPass: function() {
      return ((this.action === 'edit' && this.form.change_pwd === 0) ? '!@#$%^&*' : 'user_password')
    }
  },
  mounted() {
    window.addEventListener('resize', () => {
      return (() => {
        this.formLabelWidth = document.documentElement.clientWidth * 0.205
      })()
    }, false)
  },
  methods: {
    addUserHandle(value, action) {
      this.dialogVisible = true
      this.action = action
      this.form.user_id = value.UserID
    },
    handleChecked(value) {
      if (this.action === 'edit' && value === 0) {
        this.$refs.ruleForm.resetFields()
      } else if (this.action === 'edit' && value === 1) {
        this.$refs.ruleForm.resetFields()
      }
    },
    submitForm() {
      this.$refs.ruleForm.validate(val => {
        if (!val) return
        this.dialogVisible = false
        delete this.form['!@#$%^&*']
        this.$refs.ruleForm.resetFields()
      })
    },
    resetForm() {
      this.dialogVisible = false
      this.$refs.ruleForm.resetFields()
    },
    editUserHandle(value, action) {
      this.dialogVisible = true
      this.action = action
      this.form.user_id = value.UserID
    },
    deleteUserHandle(value) {
      // console.log(2223333, value.UserID)
      this.$confirm(this.$t('configuration.userManageConfig.comfirmMsg'), this.$t('configuration.userManageConfig.comfirmTitle'), {
        confirmButtonText: this.$t('configuration.userManageConfig.confirmButtonText'),
        cancelButtonText: this.$t('configuration.userManageConfig.cancelButtonText'),
        type: 'warning'
      }).then(() => {
        this.$message({
          type: 'success',
          message: this.$t('configuration.userManageConfig.successMsg')
        })
      }).catch(() => {
        this.$message({
          type: 'info',
          message: this.$t('configuration.userManageConfig.cancelMsg')
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
::v-deep {
  .el-dialog {
    margin-bottom: 10vh;
    margin-top: 5vh !important;
  }
  .el-dialog__header {
    margin-left: 15px;
    // padding: 15px 20px 0 20px;
    font-weight: 600;
  }
  .el-dialog__body {
    padding: 0 20px;
  }
  .el-dialog__footer {
    padding-top: 0;
    text-align: center;
  }
  .el-form-item__label {
    padding-right: 25px;
  }
  .el-input {
    max-width: 250px;
    // min-width: 200px !important;
  }
  .el-select {
    width: 100%;
  }
}
.userIdStyle {
  // font-size: 16px;
  font-weight: 700;
}
</style>

<template>
  <div class="app-container">
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('fanPolicyUpdate.fanPolicyUpdateTitle') }}</span>
        </div>
        <div>
          <div class="stepStyle">
            <el-steps :active="activeStep" simple>
              <el-step :title="$t('fanPolicyUpdate.step1')" icon="el-icon-folder-opened" />
              <el-step :title="$t('fanPolicyUpdate.step2')" icon="el-icon-upload" />
              <el-step :title="$t('fanPolicyUpdate.step3')" icon="el-icon-folder-checked" />
            </el-steps>
          </div>
          <div class="editor-container">
            <div v-if="activeStep !== 3">
              <el-upload
                ref="upload"
                drag
                action=""
                :file-list="fileList"
                :show-file-list="fileType"
                :auto-upload="false"
                :on-change="handleChange"
                :on-remove="handleRemove"
                :before-remove="beforeRemove"
              >
                <i v-if="activeStep === 0" class="el-icon-upload" />
                <div v-if="activeStep === 0" slot="tip" class="el-upload__tip">{{ $t('fanPolicyUpdate.uploadTip') }}</div>
              </el-upload>
              <div v-if="activeStep === 1 && uploadStatus === 'ready'">
                <el-button type="primary" @click="submitUpload">{{ $t('fanPolicyUpdate.step3') }}</el-button>
              </div>
            </div>
            <div v-if="activeStep === 3">
              <div v-if="uploadStatus === 'success'">
                <el-button type="success" icon="el-icon-check" circle />
                <div style="margin-top: 10px; color: #13CE66">{{ $t('fanPolicyUpdate.success') }}</div>
                <div class="backStyle">
                  <i class="el-icon-back" />
                  <span @click="backTo">{{ $t('fanPolicyUpdate.return') }}</span>
                </div>
              </div>
              <div v-else-if="uploadStatus === 'fail'">
                <el-button type="danger" icon="el-icon-close" circle />
                <div style="margin-top: 10px; color: #ff4949">{{ $t('fanPolicyUpdate.error') }}</div>
                <div class="backStyle">
                  <i class="el-icon-back" />
                  <span @click="backTo">{{ $t('fanPolicyUpdate.return') }}</span>
                </div>
              </div>
              <div v-else>
                <div v-loading="loading" style="margin-top: 75px; color: #777">{{ $t('fanPolicyUpdate.loading') }}</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </el-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      activeStep: 0,
      uploadStatus: '',
      fileType: true,
      fileList: [],
      loading: true
    }
  },
  mounted() {
  },
  methods: {
    handleChange(file, fileList) {
      console.log(fileList)
      this.uploadStatus = file.status
      if (file.name !== 'FSC_temp.dat') {
        this.$message.error(this.$t('fanPolicyUpdate.uploadTip'))
        this.fileType = false
        this.fileList = []
        return
      } else {
        this.fileType = true
        this.fileList = []
        this.fileList.push(fileList[fileList.length - 1])
      }
      if (this.uploadStatus === 'ready') {
        this.activeStep = 1
      } else if (this.uploadStatus === 'fail' || this.uploadStatus === 'success') {
        this.activeStep = 3
        if (this.uploadStatus === 'fail') {
          this.$message.error(this.$t('fanPolicyUpdate.error'))
          this.fileList = []
          setTimeout(() => {
            this.activeStep = 0
          }, 3000)
        } else {
          this.$message.success(this.$t('fanPolicyUpdate.success'))
          this.fileList = []
        }
      }
    },
    handleRemove(file, fileList) {
      this.fileList = []
      setTimeout(() => {
        this.activeStep = 0
      }, 530)
    },
    beforeRemove(file) {
      return this.$confirm(`${this.$t('fanPolicyUpdate.removeTip')} ${file.name} ?`)
    },
    backTo() {
      this.activeStep = 0
    },
    submitUpload() {
      this.$refs.upload.submit()
      this.activeStep = 3
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
  }

.infoBox {
  padding: 15px;
  background-color: #fff;
}
.infoBoxHeader {
  display: inline-block;
  span {
    // padding-left: 12px;
    font-size: 20px;
    font-weight: 600;
  }
}
.stepStyle {
    padding: 20px 0;
}
.editor-container {
    border: 2px solid #E5E5E5;
    font-family: 'Roboto', sans-serif;
    color: #777;
    transition: background-color .2s linear;
    height: 188px;
    background-color: rgb(245, 245, 245);
    text-align: center;
}
::v-deep {
    .el-upload {
        margin-top: 15px;
    }
    .el-upload-dragger {
        background-color: rgb(245, 245, 245);
        border: 0;
        border-radius: 0;
        -webkit-box-sizing: border-box;
        box-sizing: border-box;
        // width: 360px;
        // height: 110px;
        // margin-top: 10px;
        text-align: center;
        cursor: pointer;
        position: relative;
        overflow: hidden;
        display: inline;
    }
    .el-upload-list {
       margin-top: 30px;
    }
    .el-upload-list__item-name {
        margin-right: 0;
    }
    .el-button--medium {
        margin-top: 55px;
    }
    [class^=el-icon-], [class*=" el-icon-"] {
        font-weight: 600;
    }
    .el-upload-list__item .is-ready{
      border: 0;
    }
    .el-upload-list__item .el-icon-close {
      display: inline !important;
      opacity: 1;
    }
    .el-upload__tip {
      margin-top: 20px;
      color: #777;
    }
    .el-loading-mask {
      background-color: rgb(245, 245, 245, 0.2);
    }
}
.backStyle {
  margin-top: 28px;
  margin-left: -20px;
  color: #aaa;
  font-weight: 600;
  span {
    margin-left: 7px;
    font-size: 14px;
    cursor: pointer;
  }
}
</style>

<template>
  <div class="app-container">
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('download.downloadTitle') }}</span>
        </div>
        <div class="downloadStyle">
          <el-button type="primary" size="mini" plain @click="downloadHandle">{{ $t('download.downloadBtn') }}</el-button>
        </div>
      </div>
    </el-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
    }
  },
  computed: {
  },
  methods: {
    downloadHandle() {
      console.log('download')
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
.downloadStyle {
  margin: 25px 0 0 0;
}
</style>

<template>
  <div class="app-container">
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('hddInfo.hddTitle') }}</span>
        </div>
        <el-table :data="list" style="width: 100%;padding-top: 10px;">
          <el-table-column :label="$t('hddInfo.hddBack')" width="150">
            <template slot-scope="scope">{{ scope.row.DeviceTypeName }}</template>
          </el-table-column>
          <el-table-column :label="$t('hddInfo.hddSlot')" width="150">
            <template slot-scope="scope">{{ scope.row.DeviceIndex }}</template>
          </el-table-column>
          <el-table-column :label="$t('hddInfo.hddType')" width="150">
            <template slot-scope="scope">{{ scope.row.DevicePresentStatus }}</template>
          </el-table-column>
          <el-table-column :label="$t('hddInfo.hddPresent')" width="150">
            <template slot-scope="scope">{{ scope.row.DevicePresentStatus === 1 ? $t('deviceInfo.present'): $t('deviceInfo.absent') }}</template>
          </el-table-column>
          <el-table-column :label="$t('hddInfo.hddError')">
            <template slot-scope="scope">{{ scope.row.DeviceDetail }}</template>
          </el-table-column>
        </el-table>
      </div>
    </el-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      list: []
    }
  },
  mounted() {},
  methods: {}
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
.el-row {
  background-color: rgb(240, 242, 245);
}
.infoBox {
  padding: 15px;
  background-color: #fff;
}
.infoBoxHeader {
  display: inline-block;
  span {
    /*padding-left: 12px;*/
    font-size: 18px;
    font-weight: 600;
  }
}
</style>

<template>
  <div class="app-container">
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('deviceInfo.deviceTitle') }}</span>
        </div>
        <el-table :data="list" style="width: 100%;padding-top: 10px;">
          <el-table-column :label="$t('deviceInfo.typeName')" width="110">
            <template slot-scope="scope">{{ scope.row.DeviceTypeName }}</template>
          </el-table-column>
          <el-table-column :label="$t('deviceInfo.number')" width="100">
            <template slot-scope="scope">{{ scope.row.DeviceIndex }}</template>
          </el-table-column>
          <el-table-column :label="$t('deviceInfo.status')" width="150">
            <template slot-scope="scope">{{ scope.row.DevicePresentStatus === 1 ? $t('deviceInfo.present'): $t('deviceInfo.absent') }}</template>
          </el-table-column>
          <el-table-column :label="$t('deviceInfo.detail')">
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
      list: [
        {
          'DeviceTypeName': 'CPU',
          'DeviceIndex': 0,
          'DevicePresentStatus': 0,
          'DeviceDetail': '4 Cores 2.4GHz AMD EPYC 7351 16-Core Processor  0'
        },
        {
          'DeviceTypeName': 'CPU',
          'DeviceIndex': 1,
          'DevicePresentStatus': 1,
          'DeviceDetail': '4 Cores 2.4GHz AMD EPYC 7351 16-Core Processor  1'
        },
        {
          'DeviceTypeName': 'CPU',
          'DeviceIndex': 2,
          'DevicePresentStatus': 1,
          'DeviceDetail': '4 Cores 2.4GHz AMD EPYC 7351 16-Core Processor  2'
        },
        {
          'DeviceTypeName': 'DIMM',
          'DeviceIndex': 0,
          'DevicePresentStatus': 1,
          'DeviceDetail': 'CPU0 ChannelA0 DDR4 2400MHz 16GB Samsung PN:M393A1G40DB0-CPB SN:01DAB3DB'
        },
        {
          'DeviceTypeName': 'DIMM',
          'DeviceIndex': 1,
          'DevicePresentStatus': 1,
          'DeviceDetail': 'CPU0 ChannelA0 DDR4 2400MHz 16GB Samsung PN:M393A1G40DB0-CPB SN:01DAB3DB'
        },
        {
          'DeviceTypeName': 'DIMM',
          'DeviceIndex': 2,
          'DevicePresentStatus': 1,
          'DeviceDetail': 'CPU0 ChannelA0 DDR4 2400MHz 16GB Samsung PN:M393A1G40DB0-CPB SN:01DAB3DB'
        },
        {
          'DeviceTypeName': 'HDD',
          'DeviceIndex': 0,
          'DevicePresentStatus': 1,
          'DeviceDetail': '4000GB AHCI Kingston SV300S37A\/120G SN:AA17105827014275 FW:p0617a'
        },
        {
          'DeviceTypeName': 'HDD',
          'DeviceIndex': 1,
          'DevicePresentStatus': 1,
          'DeviceDetail': '4000GB AHCI Kingston SV300S37A\/120G SN:AA17105827014275 FW:p0617a'
        },
        {
          'DeviceTypeName': 'HDD',
          'DeviceIndex': 2,
          'DevicePresentStatus': 1,
          'DeviceDetail': '4000GB AHCI Kingston SV300S37A\/120G SN:AA17105827014275 FW:p0617a'
        }
      ]
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

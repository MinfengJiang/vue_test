<template>
  <div class="app-container">
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('systemPostCode.systemPostCodeTitle') }}</span>
        </div>
        <el-table :data="list" style="width: 100%;padding-top: 10px;">
          <el-table-column :label="$t('systemPostCode.currentPostCode')" width="150px" align="center" header-align="left">
            <template slot-scope="scope">
              <el-tag>{{ scope.row.CurrentPostCode }}</el-tag>
            </template>
          </el-table-column>
          <el-table-column :label="$t('systemPostCode.postCodeHistory')">
            <template slot-scope="scope">{{ scope.row.HistoryPostCode }}</template>
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
      list: [],
      data: {
        'CurrentPostCode': 'D5',
        'HistoryPostCode': ' 01 02 03 04 05 06 A1 A3 A3 A7 A9 AA AB AF B0 B0 B1 B1 AF 02 03 04 05 06 A1 A3 A3 A7 A9 A7 A7 A7 A8 A9 AA AE AF E1 E4 E3 E5 B0 B0 B1 B1 B4 B2 B3 B3 B3 B6 B6 B6 B6 B7 B7 BE B7 B7 B7 B8 B8 B8 B8 B8 B8 B8 B9 B9 BA B9 BB BB BB BB BB BB BB BB BB BB B7 BC BC BC BC BC BC BF E7 E8 E9 EB EC ED EE D1 D2 D3 D4 EF D5'
      }
    }
  },
  mounted() {
    this.getList()
  },
  methods: {
    getList() {
      this.list[0] = this.data
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

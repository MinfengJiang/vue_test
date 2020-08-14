<template>
  <div class="app-container">
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('fruInfo.sysFruTitle') }}</span>
        </div>
        <el-table :data="newObj.list0" style="width: 100%;padding-top: 10px;">
          <el-table-column :label="$t('fruInfo.tableTitle')" min-width="200">
            <template slot-scope="scope">{{ $t(`fruInfo.${scope.row.Title}`) }}</template>
          </el-table-column>
          <el-table-column :label="$t('fruInfo.sysFruTitle')" min-width="200">
            <template slot-scope="scope">{{ scope.row.value }}</template>
          </el-table-column>
        </el-table>
      </div>
    </el-row>
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('fruInfo.sysFruTitle2') }}</span>
        </div>
        <el-table :data="newObj.list1" style="width: 100%;padding-top: 10px;">
          <el-table-column :label="$t('fruInfo.tableTitle')" min-width="200">
            <template slot-scope="scope">{{ $t(`fruInfo.${scope.row.Title}`) }}</template>
          </el-table-column>
          <el-table-column :label="$t('fruInfo.sysFruTitle2')" min-width="200">
            <template slot-scope="scope">{{ scope.row.value }}</template>
          </el-table-column>
        </el-table>
      </div>
    </el-row>
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('fruInfo.sysFruTitle3') }}</span>
        </div>
        <el-table :data="newObj.list2" style="width: 100%;padding-top: 10px;">
          <el-table-column :label="$t('fruInfo.tableTitle')" min-width="200">
            <template slot-scope="scope">{{ $t(`fruInfo.${scope.row.Title}`) }}</template>
          </el-table-column>
          <el-table-column :label="$t('fruInfo.sysFruTitle3')" min-width="200">
            <template slot-scope="scope">{{ scope.row.value }}</template>
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
      newObj: {
        list0: [],
        list1: [],
        list2: []
      },
      list: [
        {
          Title: 'Chassis Information',
          ChassisType: 17,
          ChassisSerial: '123456789012345678901234567890'
        },
        {
          Title: 'Board Information',
          BoardMfgDate: '02/10/2007 23:31:00',
          BoardMfg: 'Lenovo',
          BoardProduct: 'System Board',
          BoardSerial: '123456789000000000000000000000',
          BoardPartNumber: '123456789059000000309889b63098'
        },
        {
          Title: 'Product Information',
          ProductManufacturer: 'Lenovo',
          ProductName: 'Lenovo Jintide',
          ProductPartNumber: '333333333333313336333433363335',
          ProductSerial: '123456789012345678901234567890',
          ProductAssetTag: '33333364653634363533333333333333'
        }
      ]
    }
  },
  mounted() {
    this.getList()
  },
  methods: {
    getList() {
      for (var i = 0; i < this.list.length; i++) {
        const arr = []
        for (var k in this.list[i]) {
          if (k !== 'Title') {
            const obj = {}
            obj.Title = k
            obj.value = this.list[i][k]
            arr.push(obj)
          }
        }
        const name = `list${i}`
        this.newObj[name] = arr
      }
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
  margin-bottom: 30px;
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
    font-size: 20px;
    font-weight: 600;
  }
}
</style>

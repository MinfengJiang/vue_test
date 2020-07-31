<template>
  <div class="app-container">
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('fruInfo.sysFruTitle') }}</span>
        </div>
        <el-table :data="list2" style="width: 100%;padding-top: 10px;">
          <el-table-column :label="$t('fruInfo.tableTitle')" min-width="200">
            <template slot-scope="scope">
              {{ scope.row.Title }}
            </template>
          </el-table-column>
          <el-table-column :label="$t('fruInfo.sysFruTitle')" min-width="200">
            <template slot-scope="scope">
              {{ scope.row.value }}
            </template>
          </el-table-column>
        </el-table>
      </div>
    </el-row>
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('fruInfo.sysFruTitle2') }}</span>
        </div>
        <el-table :data="list" style="width: 100%;padding-top: 10px;">
          <el-table-column :label="$t('fruInfo.tableTitle')" min-width="200">
            <template slot-scope="scope">
              {{ scope.row }}
            </template>
          </el-table-column>
          <el-table-column :label="$t('fruInfo.sysFruTitle2')" min-width="200">
            <template slot-scope="scope">
              {{ scope.row.Name }}
            </template>
          </el-table-column>
        </el-table>
      </div>
    </el-row>
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('fruInfo.sysFruTitle3') }}</span>
        </div>
        <el-table :data="list" style="width: 100%;padding-top: 10px;">
          <el-table-column :label="$t('fruInfo.tableTitle')" min-width="200">
            <template slot-scope="scope">
              {{ scope.row.Title }}
            </template>
          </el-table-column>
          <el-table-column :label="$t('fruInfo.sysFruTitle3')" min-width="200">
            <template slot-scope="scope">
              {{ scope.row.Name }}
            </template>
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
      list2: [],
      list: [
        {
          ChassisSerialNumber: '123456789012345678901234567890',
          ChassisType: 17,
          Title: 'Chassis Information',
          UUID: '30333033303330333333333333333333'
        },
        {
          BoardManufactureName: 'Lenovo',
          BoardPartNumber: '123456789059000000309889b63098',
          BoardProductName: 'System Board',
          BoardSerialNumber: '123456789000000000000000000000',
          FRUNumber: '3634363533363',
          HWRevisionLevel: '3030333039383839623633303938c0',
          MfgDateTime: '02/10/2007 23:31:00',
          POSID: 65535,
          POSIDExtention: 65535,
          PhysicalCharacteristics: '3030303030',
          SystemMAC1: '30:30:30:30:30:30',
          SystemMAC2: '30:30:30:30:30:30',
          SystemMAC3: '30:30:30:30:30:30',
          SystemMAC4: '30:33:31:35:30:34',
          Title: 'Board Information',
          VPDID: 65535,
          VPDType: 51
        }
      ]
    }
  },
  mounted() {
    this.getList()
  },
  methods: {
    getList() {
      const arr = []
      for (var i = 0; i < this.list.length; i++) {
        for (var k in this.list[i]) {
          const obj = {}
          obj.Title = k
          obj.value = this.list[i][k]
          arr.push(obj)
        }
        this.list2 = arr
      }
      console.log(this.list2)
    }
  }
}
</script>

<style lang="scss" scoped>
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
      font-size: 18px;
      font-weight: 600;
    }
  }
</style>

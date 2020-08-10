<template>
  <div class="app-container">
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('eventLog.eventLogTitle') }}</span>
          <el-button type="primary" plain size="mini">{{ $t('eventLog.btn1') }}</el-button>
          <el-button type="primary" plain size="mini">{{ $t('eventLog.btn2') }}</el-button>
        </div>
        <div class="searchTable">
          <span><el-button type="info" plain size="mini">{{ $t('eventLog.btn3') }}</el-button></span>
          <span style="float: right">
            <el-input
              v-model="inputVal"
              clearable
              size="small"
            />
          </span>
        </div>
        <el-table :data="list" style="width: 100%;padding-top: 10px;" :default-sort="{prop: 'date', order: 'descending'}">
          <el-table-column :label="$t('eventLog.Num')" width="60" prop="id" sortable>
            <template slot-scope="scope">{{ scope.row.Num }}</template>
          </el-table-column>
          <el-table-column :label="$t('eventLog.Owner')" width="130" prop="Owner" sortable>
            <template slot-scope="scope">{{ scope.row.Owner }}</template>
          </el-table-column>
          <el-table-column :label="$t('eventLog.WarningLevel')" width="140" header-align="center" align="center" prop="id" sortable>
            <template slot-scope="scope">
              <i v-if="scope.row.WarningLevel === 15" class="el-icon-error" style="color: #F54545" />
              <i v-else-if="scope.row.WarningLevel === 1" class="el-icon-info" style="color: #0073B7" />
              <i v-else class="el-icon-warning" style="color: #FFAC38" />
            </template>
          </el-table-column>
          <el-table-column :label="$t('eventLog.Time')" width="150" sortable header-align="center" prop="SensorType" :filters="filterData" :filter-method="filterHandler" filter-placement="bottom">
            <template slot-scope="scope">{{ scope.row.Time }}</template>
          </el-table-column>
          <el-table-column :label="$t('eventLog.Name')" width="150" prop="Name" sortable>
            <template slot-scope="scope">{{ scope.row.Name }}</template>
          </el-table-column>
          <el-table-column :label="$t('eventLog.Type')" width="130" prop="Type" sortable>
            <template slot-scope="scope">{{ scope.row.Type }}</template>
          </el-table-column>
          <el-table-column :label="$t('eventLog.Description')" prop="Description" sortable>
            <template slot-scope="scope">{{ scope.row.Description }}</template>
          </el-table-column>
        </el-table>
      </div>
    </el-row>
    <el-row>
      <el-col class="tipStyle" :xs="24" :sm="8" :md="8" :lg="8" :xl="6">
        <span><i class="el-icon-info" style="color: #0073B7" />{{ $t('eventLog.Info') }}</span>
        <span><i class="el-icon-warning" style="color: #FFAC38" />{{ $t('eventLog.Warning') }}</span>
        <span><i class="el-icon-error" style="color: #F54545" />{{ $t('eventLog.Error') }}</span>
      </el-col>
      <el-col class="pageStyle" :xs="24" :sm="16" :md="16" :lg="16" :xl="18">
        <el-pagination
          :current-page="currentPage4"
          :page-sizes="[100, 200, 300, 400]"
          :page-size="100"
          :layout="paginationLayout"
          :total="400"
          background
          :small="isSmall"
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
        />
      </el-col>
    </el-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      list: [
        {
          'Num': 5,
          'Owner': 'BMC',
          'Time': '2020/08/04 02:42:16',
          'Name': 'FAN7_Presence',
          'Type': 'Physical Security',
          'Description': "{{'State Asserted'|translate}} - {{'Deasserted'|translate}}",
          'WarningLevel': 15
        },
        {
          'Num': 4,
          'Owner': 'IPMB Slave 0x08',
          'Time': 'Pre-Init 0000002065',
          'Name': 'sensor number 0x12',
          'Type': 'Watchdog1',
          'Description': "{{'Unable to parse SEL raw data'|translate}} 14 15 16 - {{'Asserted'|translate}}",
          'WarningLevel': 1
        },
        {
          'Num': 3,
          'Owner': 'IPMB Slave 0x08',
          'Time': 'Pre-Init 0000002061',
          'Name': 'sensor number 0x12',
          'Type': 'Watchdog1',
          'Description': "{{'Unable to parse SEL raw data'|translate}} 14 15 16 - {{'Asserted'|translate}}",
          'WarningLevel': 0
        },
        {
          'Num': 2,
          'Owner': 'OEM',
          'Time': 'non-timestamped',
          'Name': 'OEM',
          'Type': 'OEM',
          'Description': 'OEM record type e1 Raw data:04050607080910111213141516',
          'WarningLevel': 1
        },
        {
          'Num': 1,
          'Owner': 'IPMB Slave 0x08',
          'Time': 'Pre-Init 0000002066',
          'Name': 'sensor number 0x12',
          'Type': 'Watchdog1',
          'Description': "{{'Unable to parse SEL raw data'|translate}} 14 15 16 - {{'Asserted'|translate}}",
          'WarningLevel': 2
        },
        {
          'Num': 0,
          'Owner': 'BMC',
          'Time': 'Pre-Init 0000001918',
          'Name': 'System Event Log',
          'Type': 'Event Logging Disabled',
          'Description': "{{'Log area reset\/cleared'|translate}} - {{'Asserted'|translate}}",
          'WarningLevel': 0
        }
      ],
      currentPage4: 1,
      paginationLayout: '',
      isSmall: false,
      screenWidth: document.documentElement.clientWidth,
      inputVal: ''
    }
  },
  computed: {
    filterData: function() {
      return [
        { text: this.$t(`sensorReading['OEM']`), value: 'OEM' },
        { text: this.$t(`sensorReading['reserved']`), value: 'reserved' },
        { text: this.$t(`sensorReading['Temperature']`), value: 'Temperature' },
        { text: this.$t(`sensorReading['Voltage']`), value: 'Voltage' },
        { text: this.$t(`sensorReading['Current']`), value: 'Current' },
        { text: this.$t(`sensorReading['Fan']`), value: 'Fan' },
        { text: this.$t(`sensorReading['Physical Security']`), value: 'Physical Security' },
        { text: this.$t(`sensorReading['Platform Security']`), value: 'Platform Security' },
        { text: this.$t(`sensorReading['Processor']`), value: 'Processor' },
        { text: this.$t(`sensorReading['Power Supply']`), value: 'Power Supply' },
        { text: this.$t(`sensorReading['Power Unit']`), value: 'Power Unit' },
        { text: this.$t(`sensorReading['Cooling Device']`), value: 'Cooling Device' },
        { text: this.$t(`sensorReading['Other']`), value: 'Other' },
        { text: this.$t(`sensorReading['Memory']`), value: 'Memory' },
        { text: this.$t(`sensorReading['Drive Slot / Bay']`), value: 'Drive Slot / Bay' },
        { text: this.$t(`sensorReading['POST Memory Resize']`), value: 'POST Memory Resize' },
        { text: this.$t(`sensorReading['System Firmwares']`), value: 'System Firmwares' },
        { text: this.$t(`sensorReading['Event Logging Disabled']`), value: 'Event Logging Disabled' },
        { text: this.$t(`sensorReading['Watchdog1']`), value: 'Watchdog1' },
        { text: this.$t(`sensorReading['System Event']`), value: 'System Event' },
        { text: this.$t(`sensorReading['Critical Interrupt']`), value: 'Critical Interrupt' },
        { text: this.$t(`sensorReading['Button']`), value: 'Button' },
        { text: this.$t(`sensorReading['Module / Board']`), value: 'Module / Board' },
        { text: this.$t(`sensorReading['Microcontroller']`), value: 'Microcontroller' },
        { text: this.$t(`sensorReading['Add-in Card']`), value: 'Add-in Card' },
        { text: this.$t(`sensorReading['Chassis']`), value: 'Chassis' },
        { text: this.$t(`sensorReading['Chip Set']`), value: 'Chip Set' },
        { text: this.$t(`sensorReading['Other FRU']`), value: 'Other FRU' },
        { text: this.$t(`sensorReading['Cable / Interconnect']`), value: 'Cable / Interconnect' },
        { text: this.$t(`sensorReading['Terminator']`), value: 'Terminator' },
        { text: this.$t(`sensorReading['System Boot Initiated']`), value: 'System Boot Initiated' },
        { text: this.$t(`sensorReading['Boot Error']`), value: 'Boot Error' },
        { text: this.$t(`sensorReading['OS Boot']`), value: 'OS Boot' },
        { text: this.$t(`sensorReading['OS Critical Stop']`), value: 'OS Critical Stop' },
        { text: this.$t(`sensorReading['Slot / Connector']`), value: 'Slot / Connector' },
        { text: this.$t(`sensorReading['System ACPI Power State']`), value: 'System ACPI Power State' },
        { text: this.$t(`sensorReading['Watchdog2']`), value: 'Watchdog2' },
        { text: this.$t(`sensorReading['Platform Alert']`), value: 'Platform Alert' },
        { text: this.$t(`sensorReading['Entity Presence']`), value: 'Entity Presence' },
        { text: this.$t(`sensorReading['Monitor ASIC']`), value: 'Monitor ASIC' },
        { text: this.$t(`sensorReading['LAN']`), value: 'LAN' },
        { text: this.$t(`sensorReading['Management Subsys Health']`), value: 'Management Subsys Health' },
        { text: this.$t(`sensorReading['Battery']`), value: 'Battery' },
        { text: this.$t(`sensorReading['Session Audit']`), value: 'Session Audit' },
        { text: this.$t(`sensorReading['Version Change']`), value: 'Version Change' },
        { text: this.$t(`sensorReading['FRU State']`), value: 'FRU State' }
      ]
    }
  },
  mounted() {
    this.changePaginationStyle()
    window.addEventListener('resize', () => {
      return (() => {
        this.screenWidth = document.documentElement.clientWidth
        this.changePaginationStyle()
      })()
    }, false)
  },
  methods: {
    changePaginationStyle() {
      // console.log('screenWidth', this.screenWidth)
      if (this.screenWidth <= 400) {
        this.isSmall = true
        this.paginationLayout = 'prev, pager, next'
      } else if (this.screenWidth > 400 && this.screenWidth <= 595) {
        this.isSmall = true
        this.paginationLayout = 'total, prev, pager, next'
      } else if (this.screenWidth > 595 && this.screenWidth <= 715) {
        this.isSmall = false
        this.paginationLayout = 'total, prev, pager, next'
      } else if (this.screenWidth > 715 && this.screenWidth <= 765) {
        this.isSmall = false
        this.paginationLayout = 'total, sizes, prev, pager, next'
      } else if (this.screenWidth > 765 && this.screenWidth <= 810) {
        this.isSmall = false
        this.paginationLayout = 'prev, pager, next'
      } else if (this.screenWidth > 810 && this.screenWidth <= 1070) {
        this.isSmall = false
        this.paginationLayout = 'total, prev, pager, next'
      } else {
        this.isSmall = false
        this.paginationLayout = 'total, sizes, prev, pager, next, jumper'
      }
    },
    filterHandler(value, row, column) {
      const property = column['property']
      return row[property] === value
    },
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`)
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`)
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
  // background-color: rgb(240, 242, 245);
  background-color: #fff;
  min-width: 310px !important;
}
.infoBox {
  padding: 15px;
  background-color: #fff;
}
.infoBoxHeader {
  display: flex;
  span {
    font-size: 18px;
    font-weight: 600;
    flex: 5;
  }
  .el-select {
      flex: 1;
      .el-input--medium .el-input__inner {
        height: 20px;
        line-height: 20px;
    }
  }
  .el-button {
    font-weight: 600;
    letter-spacing: 1px;
    padding: 7px 8px;
  }
}
@media screen and (max-width: 765px) {
  .pageStyle {
    min-width: 310px !important;
    .el-pagination {
      margin: 0 15px 20px 12px !important;
      float: none !important;
    }
  }
}
.pageStyle {
  min-width: 310px !important;
  .el-pagination {
    margin: 0 15px 20px 0;
    float: right;
  }
}
.tipStyle {
  // min-width: 310px !important;
  padding: 8px 0 15px 10px;
  font-size: 14px;
  font-weight: 600;
  color: #666;
  span {
    padding-left: 8px;
    letter-spacing: 1px;
    i {
      padding-right: 3px;
    }
  }
}
.searchTable {
  padding: 15px 0 0 0;
  .el-button {
    font-weight: 600;
    letter-spacing: 1px;
    padding: 7px 8px;
  }
  .el-input {
    position: relative;
    font-size: 14px;
    display: inline-block;
    width: 150px !important;
  }
}
</style>

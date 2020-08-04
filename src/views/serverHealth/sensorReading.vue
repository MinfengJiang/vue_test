<template>
  <div class="app-container">
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('sensorReading.sensorReadingTitle') }}</span>
          <el-select v-model="value" :placeholder="$t('sensorReading.placeholder')">
            <el-option
              v-for="item in options"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            />
          </el-select>
        </div>
        <el-table :data="list" style="width: 100%;padding-top: 10px;">
          <el-table-column :label="$t('sensorReading.name')" width="150">
            <template slot-scope="scope">{{ scope.row.SensorName }}</template>
          </el-table-column>
          <el-table-column :label="$t('sensorReading.reading')" width="150">
            <template slot-scope="scope">{{ scope.row.SensorReading }}</template>
          </el-table-column>
          <el-table-column :label="$t('sensorReading.sensorType')" width="180" prop="SensorType" :filters="filterData" :filter-method="filterHandler" filter-placement="bottom">
            <template slot-scope="scope">{{ $t(`sensorReading['${scope.row.SensorType}']`) }}</template>
          </el-table-column>
          <el-table-column :label="$t('sensorReading.unit')" width="150">
            <template slot-scope="scope">{{ $t(`sensorReading['${scope.row.Unit}']`) }}</template>
          </el-table-column>
          <el-table-column :label="$t('sensorReading.status')">
            <template slot-scope="scope">{{ $t(`sensorReading['${scope.row.SensorState}']`) }}</template>
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
          'SensorName': 'CPU0_Temp',
          'SensorNum': 1,
          'EventReadingType': 1,
          'SensorType': 'Temperature',
          'SensorReading': 60,
          'Unit': 'degrees C',
          'SensorState': 'Normal'
        },
        {
          'SensorName': 'Inlet_Temp',
          'SensorNum': 2,
          'EventReadingType': 1,
          'SensorType': 'Temperature',
          'SensorReading': 40.1234567,
          'Unit': 'degrees C',
          'SensorState': 'Upper Critical - going high'
        },
        {
          'SensorName': 'CPU0_Status',
          'SensorNum': 20,
          'EventReadingType': 111,
          'SensorType': 'Processor',
          'SensorReading': 8002,
          'Unit': 'NA',
          'SensorState': 'Thermal Trip'
        },
        {
          'SensorName': 'PSU1_Status',
          'SensorNum': 40,
          'EventReadingType': 111,
          'SensorType': 'Power Supply',
          'SensorReading': 8010,
          'Unit': 'NA',
          'SensorState': 'Power Supply input lost or out-of-range'
        },
        {
          'SensorName': 'Mytest',
          'SensorNum': 40,
          'EventReadingType': 111,
          'SensorType': 'Power Supply',
          'SensorReading': 8010,
          'Unit': 'NA',
          'SensorState': 'Disabled'
        }
      ],
      value: ''
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
    },
    options: function() {
      if (this.$i18n.locale === 'zh') {
        return [
          { value: '0', label: this.$t(`sensorReading['Reading Period Disabled']`) },
          { value: '1', label: `5${this.$t(`sensorReading['Reading Period']`)} (5s)` },
          { value: '2', label: `10${this.$t(`sensorReading['Reading Period']`)} (10s)` },
          { value: '3', label: `30${this.$t(`sensorReading['Reading Period']`)} (30s)` },
          { value: '4', label: `60${this.$t(`sensorReading['Reading Period']`)} (60s)` }
        ]
      } else {
        return [
          { value: '0', label: this.$t(`sensorReading['Reading Period Disabled']`) },
          { value: '1', label: `${this.$t(`sensorReading['Reading Period']`)}5 seconds (5s)` },
          { value: '2', label: `${this.$t(`sensorReading['Reading Period']`)}10 seconds (10s)` },
          { value: '3', label: `${this.$t(`sensorReading['Reading Period']`)}30 seconds (30s)` },
          { value: '4', label: `${this.$t(`sensorReading['Reading Period']`)}60 seconds (60s)` }
        ]
      }
    }
  },
  methods: {
    filterHandler(value, row, column) {
      const property = column['property']
      return row[property] === value
    }
  }
}
</script>

<style lang="scss" scoped>
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
  display: flex;
  span {
    /*padding-left: 12px;*/
    font-size: 18px;
    font-weight: 600;
    flex: 5;
    // height: 36px;
    // line-height: 36px;
  }
  .el-select {
      flex: 1;
      .el-input--medium .el-input__inner {
        height: 20px;
        line-height: 20px;
    }
  }
}
</style>

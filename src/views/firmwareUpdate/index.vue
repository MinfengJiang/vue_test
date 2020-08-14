<template>
  <div class="app-container">
    <el-row><BIOS /></el-row>
    <el-row><BMC /></el-row>
    <el-row><BackupBMC /></el-row>
    <el-row><CPLD /></el-row>
  </div>
</template>

<script>
import BIOS from './components/BIOSFirmware'
import BMC from './components/BMCFirmware'
import BackupBMC from './components/backupBMCFirmware'
import CPLD from './components/CPLDFirmware'

export default {
  name: 'FirmwareUpdate',
  components: {
    BIOS,
    BMC,
    BackupBMC,
    CPLD
  },
  data() {
    return {
    }
  },
  methods: {
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
  position: relative;
  height: 100%;
  width: 100%;
  overflow: auto;
}
.el-row{
  margin-bottom: 30px;
  background-color: rgb(240, 242, 245);
  height: 100%;
}
.el-row:nth-last-child(1) {
  margin-bottom: 0;
  background-color: rgb(240, 242, 245);
  height: 100%;
}
</style>

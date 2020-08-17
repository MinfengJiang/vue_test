<template>
  <div class="app-container">
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('configuration.networkSettingConfig.networkSettingConfigTitle') }}</span>
        </div>
        <div>
          <div class="labelBox">
            <div class="labelLeft">{{ $t('configuration.networkSettingConfig.lanFail') }}</div>
            <div class="labelRight">
              <el-switch
                v-model="lan_fail_over"
                :active-value="0"
                :inactive-value="1"
                style="display: block"
                active-color="#6fc596"
                inactive-color="#ff8080"
                :active-text="$t('configuration.networkSettingConfig.lanEnable')"
                :inactive-text="$t('configuration.networkSettingConfig.lanDisable')"
              />
            </div>
          </div>
          <div class="labelBox">
            <div class="labelLeft">{{ $t('configuration.networkSettingConfig.lanInterface') }}</div>
            <div class="labelRight">
              <el-select v-model="formData.lan_channel_num" :placeholder="$t('configuration.networkSettingConfig.lanInterfacePlaceholder')">
                <el-option label="1" :value="1" />
                <el-option label="2" :value="2" />
              </el-select>
            </div>
          </div>
          <div class="labelBox">
            <div class="labelLeft">{{ $t('configuration.networkSettingConfig.macAddress') }}</div>
            <div class="labelRight">
              <el-input v-model="formData.mac_addr" autocomplete="off" :placeholder="$t('configuration.networkSettingConfig.macAddressPlaceholder')" />
            </div>
          </div>
        </div>
      </div>
    </el-row>
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('configuration.networkSettingConfig.networkSettingConfigTitle2') }}</span>
        </div>
        <div>
          <div class="labelBox">
            <div class="labelLeft">{{ $t('configuration.networkSettingConfig.autoIP') }}</div>
            <div class="labelRight">
              <el-radio v-model="formData.v4_ip_source" :label="0">{{ '' }}</el-radio>
            </div>
          </div>
          <div class="labelBox">
            <div class="labelLeft">{{ $t('configuration.networkSettingConfig.manualIP') }}</div>
            <div class="labelRight">
              <el-radio v-model="formData.v4_ip_source" :label="1">{{ '' }}</el-radio>
            </div>
          </div>
          <div class="labelBox">
            <div class="labelLeft">{{ $t('configuration.networkSettingConfig.IP') }}</div>
            <div class="labelRight">
              <el-input v-model="formData.v4_ip_addr" autocomplete="off" :disabled="formData.v4_ip_source === 0" :placeholder="$t('configuration.networkSettingConfig.IPPlaceholder')" />
            </div>
          </div>
          <div class="labelBox">
            <div class="labelLeft">{{ $t('configuration.networkSettingConfig.mask') }}</div>
            <div class="labelRight">
              <el-input v-model="formData.v4_subnet" autocomplete="off" :disabled="formData.v4_ip_source === 0" :placeholder="$t('configuration.networkSettingConfig.maskPlaceholder')" />
            </div>
          </div>
          <div class="labelBox">
            <div class="labelLeft">{{ $t('configuration.networkSettingConfig.gateway') }}</div>
            <div class="labelRight">
              <el-input v-model="formData.v4_gateway" autocomplete="off" :disabled="formData.v4_ip_source === 0" :placeholder="$t('configuration.networkSettingConfig.gatewayPlaceholder')" />
            </div>
          </div>
        </div>
      </div>
    </el-row>
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('configuration.networkSettingConfig.networkSettingConfigTitle3') }}</span>
        </div>
        <div>
          <div class="labelBox">
            <div class="labelLeft">{{ $t('configuration.networkSettingConfig.ipv6Enable') }}</div>
            <div class="labelRight">
              <el-checkbox v-model="formData.v6_enable" :true-label="0" :false-label="1" />
            </div>
          </div>
          <div class="labelBox">
            <div class="labelLeft">{{ $t('configuration.networkSettingConfig.autoIP') }}</div>
            <div class="labelRight">
              <el-radio v-model="formData.v6_ip_source" :label="0" :disabled="formData.v6_enable === 1">{{ '' }}</el-radio>
            </div>
          </div>
          <div class="labelBox">
            <div class="labelLeft">{{ $t('configuration.networkSettingConfig.manualIP') }}</div>
            <div class="labelRight">
              <el-radio v-model="formData.v6_ip_source" :label="1" :disabled="formData.v6_enable === 1">{{ '' }}</el-radio>
            </div>
          </div>
          <div class="labelBox">
            <div class="labelLeft">{{ $t('configuration.networkSettingConfig.ipv6Address') }}</div>
            <div class="labelRight">
              <el-input v-model="formData.v6_ip_addr" autocomplete="off" :disabled="formData.v6_ip_source === 0" :placeholder="$t('configuration.networkSettingConfig.ipv6AddressPlaceholder')" />
            </div>
          </div>
          <div class="labelBox">
            <div class="labelLeft">{{ $t('configuration.networkSettingConfig.addressPrefixLength') }}</div>
            <div class="labelRight">
              <el-input v-model="formData.v6_prefix_len" autocomplete="off" :disabled="formData.v6_ip_source === 0" :placeholder="$t('configuration.networkSettingConfig.addressPrefixLengthPlaceholder')" />
            </div>
          </div>
        </div>
      </div>
    </el-row>
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('configuration.networkSettingConfig.networkSettingConfigTitle4') }}</span>
        </div>
        <div>
          <div class="labelBox">
            <div class="labelLeft">{{ $t('configuration.networkSettingConfig.primaryDNS') }}</div>
            <div class="labelRight">
              <el-input v-model="formData.primary_dns" autocomplete="off" :disabled="formData.v6_ip_source === 0" :placeholder="$t('configuration.networkSettingConfig.primaryDNSPlaceholder')" />
            </div>
          </div>
          <div class="labelBox">
            <div class="labelLeft">{{ $t('configuration.networkSettingConfig.secondDNS') }}</div>
            <div class="labelRight">
              <el-input v-model="formData.second_dns" autocomplete="off" :disabled="formData.v6_ip_source === 0" :placeholder="$t('configuration.networkSettingConfig.secondDNSPlaceholder')" />
            </div>
          </div>
        </div>
      </div>
    </el-row>
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('configuration.networkSettingConfig.networkSettingConfigTitle5') }}</span>
        </div>
        <div>
          <div class="labelBox">
            <div class="labelLeft">{{ $t('configuration.networkSettingConfig.vlanEnable') }}</div>
            <div class="labelRight">
              <el-checkbox v-model="formData.vlan_enable" :true-label="0" :false-label="1" />
            </div>
          </div>
          <div class="labelBox">
            <div class="labelLeft">{{ $t('configuration.networkSettingConfig.vlanID') }}</div>
            <div class="labelRight">
              <el-input v-model.number="formData.vlan_ID" autocomplete="off" :disabled="formData.vlan_enable === 1" :placeholder="$t('configuration.networkSettingConfig.vlanIDPlaceholder')" />
            </div>
          </div>
          <div class="labelBox">
            <div class="labelLeft">{{ $t('configuration.networkSettingConfig.vlanPriority') }}</div>
            <div class="labelRight">
              <el-input v-model.number="formData.vlan_priority" autocomplete="off" :disabled="formData.vlan_enable === 1" :placeholder="$t('configuration.networkSettingConfig.vlanPriorityPlaceholder')" />
            </div>
          </div>
        </div>
      </div>
    </el-row>
    <el-row class="submitForm">
      <el-button type="primary" @click="submitForm">{{ $t('configuration.networkSettingConfig.confirmBtn') }}</el-button>
    </el-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      list: [],
      lan_fail_over: 0,
      formData: {
        lan_channel_num: 1,
        mac_addr: 'DA:42:28:09:1C:DC',
        v4_enable: 0,
        v4_ip_source: 0,
        v4_ip_addr: '192.168.100.149',
        v4_subnet: '255.255.255.0',
        v4_gateway: '192.168.100.1',
        v6_enable: 0,
        v6_ip_source: 0,
        v6_ip_addr: '234',
        v6_prefix_len: '01423',
        primary_dns: '',
        second_dns: '',
        vlan_enable: 1,
        vlan_ID: 0,
        vlan_priority: 0
      }
    }
  },
  methods: {
    submitForm() {
      console.log(this.formData)
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
    min-width: 465px;
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
.labelBox {
  margin: 15px 0;
  font-size: 14px;
  font-weight: 600;
  color: #666;
  .labelLeft {
    display: inline-block;
    width: 42%;
    text-align: right;
    padding: 10px 35px 10px 20px;
  }
  .labelRight {
    display: inline-block;
    width: 58%;
    padding: 10px 0;
  }
}
.el-switch {
  display: inline-block !important;
  margin-left: 40px;
}
.submitForm {
  padding: 15px 15px 30px 15px;
  background-color: #fff;
  flex: 1;
  text-align: right;
  padding-right: 5%;
}
::v-deep {
  .el-switch__label {
    font-weight: 600;
    color: #666;
    width: 40px;
  }
  .el-switch__label.is-active {
    color: #1890ff;
  }
  .el-switch__label--left {
    margin-right: 0;
  }
  .el-switch__core {
    margin: 0 25px;
  }
  .el-switch__label--right {
    margin-left: 0;
  }
  .el-input {
    max-width: 250px;
    // min-width: 200px !important;
  }
  .el-input--suffix .el-input__inner {
    padding-right: 75px;
  }
  // .el-select {
  //   width: 100%;
  // }
}
</style>

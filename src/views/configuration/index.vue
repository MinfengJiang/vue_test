<template>
  <div class="app-container">
    <el-row>
      <div class="infoBox">
        <!-- <div class="infoBoxHeader">
          <span>{{ $t('configuration.configurationTitle') }}</span>
        </div> -->
        <div class="configBox">
          <div v-for="item in configList" :key="item.id" class="configItem">
            <router-link :to="`/configuration/${item.name}`">
              <svg-icon :icon-class="item.name" />
              <p>{{ $t(`configuration.${item.name}`) }}</p>
            </router-link>
          </div>
        </div>
      </div>
    </el-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      configList: [
        {
          id: 1,
          name: 'userManage'
        },
        {
          id: 2,
          name: 'networkSetting'
        },
        {
          id: 3,
          name: 'timeSetting'
        },
        {
          id: 4,
          name: 'ntpSetting'
        },
        {
          id: 5,
          name: 'serivceManage'
        },
        {
          id: 6,
          name: 'ldapSetting'
        },
        {
          id: 7,
          name: 'smtpSetting'
        },
        {
          id: 8,
          name: 'restore'
        },
        {
          id: 9,
          name: 'powerOnMode'
        },
        {
          id: 10,
          name: 'powerCapping'
        },
        {
          id: 11,
          name: 'snmpSetting'
        },
        {
          id: 12,
          name: 'trapSetting'
        },
        {
          id: 13,
          name: 'sessionManage'
        },
        {
          id: 14,
          name: 'ipAccessControl'
        },
        {
          id: 15,
          name: 'bootOptionSetting'
        },
        {
          id: 16,
          name: 'dualMirrorSetting'
        }
      ]
    }
  },
  mounted() {
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
    position: absolute;
    height: 100%;
    width: 100%;
  }
.infoBox {
  padding: 15px 0 10px 18px;
  background-color: #fff;
}
.infoBoxHeader {
  display: inline-block;
  span {
    font-size: 18px;
    font-weight: 600;
  }
}
.configBox {
  margin: 15px 0 0 10px;
  *zoom: 1; // IE6清除浮动
}
 // 清除浮动
.configBox:after {
  content: "";
  display: block;
  height: 0;
  clear:both;
  visibility: hidden;
}
.configItem {
  float: left;
  width: 138px;
  height: 60px;
  line-height: 22px;
  background-color: rgb(228, 228, 228);
  border-radius: 8px;
  text-align: center;
  padding-top: 6px;
  color: #555;
  border: 1px solid rgb(218, 218, 218);
  margin: 0 23px 25px 0;
  cursor: pointer;
  .svg-icon[data-v-c8a70580] {
    width: 1.3em;
    height: 1.3em;
}
  p {
    margin-block-start: 0;
    margin-block-end: 0;
    font-size: 14px;
    font-weight: 600;
  }
}
.configItem:hover {
  background-color: rgb(217, 247, 255);
  border: 1px solid rgb(232, 232, 232);
}
</style>

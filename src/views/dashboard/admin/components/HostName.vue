<template>
  <div class="infoBox">
    <div class="infoBoxHeader">
      <i class="el-icon-s-platform" />
      <span>{{ $t('dashboard.HostName') }}</span>
    </div>
    <div class="infoStyle">{{ value }}</div>
  </div>
</template>

<script>

export default {
  data() {
    return {
      value: '1234anlkaa33f41'
    }
  },
  created() {
  },
  methods: {
  }
}
</script>

<style lang="scss" scoped>
  .infoBox {
    padding-bottom: 16px;
    background-color: #fff;
  }
  .infoBoxHeader {
    display: inline-block;
    padding-left: 5px;
    span {
      padding-left: 12px;
      font-size: 20px;
      font-weight: 600;
    }
  }
  .infoStyle {
    display: inline-block;
    float: right;
    font-size: 18px;
    font-weight: 500;
    padding-top: 2px;
  }
</style>

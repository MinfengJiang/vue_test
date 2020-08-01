<template>
  <div class="infoBox">
    <el-row>
      <el-col :xs="22" :sm="22" :md="22" :lg="21" :xl="21">
        <div class="infoBoxHeader">
          <i class="el-icon-s-opportunity" />
          <span>{{ $t('dashboard.UIDTitle') }}</span>
        </div>
      </el-col>
      <el-col :xs="2" :sm="2" :md="2" :lg="3" :xl="3">
        <el-switch v-model="value" active-color="#3CC4C4" />
      </el-col>
    </el-row>
  </div>
</template>

<script>

export default {
  data() {
    return {
      value: true
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
</style>

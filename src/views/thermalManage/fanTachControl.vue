<template>
  <div class="app-container">
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('fanTachControl.fanTachControlTitle') }}</span>
        </div>
        <el-table :data="list" style="width: 100%;padding-top: 10px;">
          <el-table-column :label="$t('fanTachControl.ID')">
            <template slot-scope="scope">{{ scope.row.FanId }}</template>
          </el-table-column>
          <el-table-column :label="$t('fanTachControl.status')">
            <template slot-scope="scope">
              <i v-if="scope.row.FanStatus === 0" class="el-icon-success" style="color: #00A65A" />
              <i v-else-if="scope.row.FanStatus === 1" class="el-icon-error" style="color: #F54545" />
              <i v-else class="el-icon-success" style="color: #aaaaaa" />
            </template>
          </el-table-column>
          <el-table-column :label="$t('fanTachControl.speed')">
            <template slot-scope="scope">{{ scope.row.FanTach }}</template>
          </el-table-column>
          <el-table-column :label="$t('fanTachControl.cycle')">
            <template slot-scope="scope">{{ scope.row.DutyCycle }}%</template>
          </el-table-column>
        </el-table>
        <div class="tipStyle">
          <span><i class="el-icon-success" style="color: #00A65A" />{{ $t('fanTachControl.success') }}</span>
          <span><i class="el-icon-error" style="color: #F54545" />{{ $t('fanTachControl.error') }}</span>
          <span><i class="el-icon-success" style="color: #aaaaaa" />{{ $t('fanTachControl.info') }}</span>
        </div>
      </div>
    </el-row>
    <el-row>
      <div class="infoBox">
        <div class="infoBoxHeader">
          <span>{{ $t('fanTachControl.fanTachControlTitle2') }}</span>
        </div>
        <div class="radioStyle">
          <el-col :xs="12" :sm="12" :md="12" :lg="12" :xl="12">
            <el-radio v-model="radio" label="1">{{ $t('fanTachControl.manual') }}</el-radio>
          </el-col>
          <el-col :xs="12" :sm="12" :md="12" :lg="12" :xl="12">
            <el-radio v-model="radio" label="2">{{ $t('fanTachControl.auto') }}</el-radio>
          </el-col>
        </div>
        <el-table :data="list2" style="width: 100%;padding-top: 10px;">
          <el-table-column :label="$t('fanTachControl.pwmId')" width="80px" align="center" header-align="left">
            <template slot-scope="scope">{{ scope.row.Id }}</template>
          </el-table-column>
          <el-table-column :label="$t('fanTachControl.fanTachControlTitle2')" min-width="250px" align="center">
            <template slot-scope="scope">
              <el-input
                :placeholder="$t('fanTachControl.placeholder')"
                :disabled="radio !== '1'"
                :min="scope.row.PwmMinValue"
                :max="scope.row.PwmMaxValue"
                @change="onInputChange"
              />
            </template>
          </el-table-column>
          <el-table-column :label="$t('fanTachControl.fanControl')" width="200px" align="center">
            <template slot-scope="scope">{{ scope.row.ControlFanName }}</template>
          </el-table-column>
        </el-table>
        <div class="btnStyle">
          <el-button type="primary" @click="submit">{{ $t('fanTachControl.submit') }}</el-button>
        </div>
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
          'FanId': 'FAN_1',
          'FanStatus': 0,
          'FanTach': 1111,
          'DutyCycle': 47
        },
        {
          'FanId': 'FAN_2',
          'FanStatus': 1,
          'FanTach': 7330,
          'DutyCycle': 47
        },
        {
          'FanId': 'FAN_3',
          'FanStatus': 2,
          'FanTach': 7402,
          'DutyCycle': 47
        },
        {
          'FanId': 'FAN_4',
          'FanStatus': 0,
          'FanTach': 7406,
          'DutyCycle': 47
        }
      ],
      list2: [
        { 'Id': 1, 'PwmMinValue': 20, 'PwmMaxValue': 100, 'ControlFanName': 'FAN1,FAN2' }, { 'Id': 2, 'PwmMinValue': 20, 'PwmMaxValue': 100, 'ControlFanName': 'FAN3,FAN4' }, { 'Id': 3, 'PwmMinValue': 20, 'PwmMaxValue': 100, 'ControlFanName': 'FAN5,FAN6' }, { 'Id': 4, 'PwmMinValue': 20, 'PwmMaxValue': 100, 'ControlFanName': 'FAN7,FAN8' }
      ],
      radio: '2',
      inputVal1: '1',
      inputVal2: '2',
      inputVal3: '3',
      inputVal4: '4',
      input: [
        {
          key: 1,
          inputVal: '1'
        },
        {
          key: 2,
          inputVal: '1'
        }
      ]
    }
  },
  mounted() {
    this.getList()
  },
  methods: {
    getList() {},
    onInputChange(e) {
      this.inputVal1 = e.target.value
    },
    submit() {
      console.log(this.radio)
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
.tipStyle {
  // min-width: 310px !important;
  padding: 15px 0 0 0;
  font-size: 14px;
  font-weight: 600;
  color: #666;
  span {
    padding-left: 20px;
    i {
      padding-right: 8px;
    }
  }
  span:nth-child(1) {
    padding-left: 0;
  }
}
.radioStyle {
  margin: 15px 0;
  text-align: center;
  .el-radio {
    font-weight: 700 !important;
  }
}
.el-radio__input.is-checked + .el-radio__label {
  color: #666 !important;
}
.el-input {
    position: relative;
    font-size: 14px;
    display: inline-block;
    width: 125px !important;
}
// 修改scope中渲染的组件的样式失效，应使用::v-deep代替/deep/
// ::v-deep {
//   .el-input--medium .el-input__inner {
//     height: 36px;
//     line-height: 36px;
//     padding: 0 0 0 55px;
//   }
// }
.btnStyle {
  margin: 15px 20px 0 0;
  flex: 1;
  text-align: right;
  .el-button {
    padding: 9px 12px 9px 14px;
    font-size: 14px;
    border-radius: 3px;
  }
}
</style>

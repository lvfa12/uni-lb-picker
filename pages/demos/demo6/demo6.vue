<template>
  <view class="list">
    <view class="grey-block"></view>
    <view class="radios">
      <radio-group class="radio-group flex-row-center"
        @change="radioChange">
        <label class="flex-row-center"
          v-for="n in radios"
          :key="n">
          <radio :value="n"
            :checked="n === columnNum"> </radio>
          <text class="item-label">{{ n }}个</text>
        </label>
      </radio-group>
    </view>
    <view class="list-item"
      @tap="handleTap('picker1')">
      <text class="sub-title">可视区自定义滚动个数：{{ columnNum }}（默认值为5）</text>
      <view class="item-content">
        <text class="item-title">绑定值:{{ value }}</text>
        <view class="item-value">
          <text class="item-label">{{ label }}</text>
          <text v-if="!label"
            class="item-placeholder">点我打开选择器</text>
        </view>
      </view>
      <lb-picker ref="picker1"
        v-model="value"
        mode="selector"
        :list="list"
        :column-num="columnNum"
        @change="handleChange"
        @confirm="handleConfirm"
        @cancel="handleCancel">
      </lb-picker>
    </view>
    <view class="grey-block"></view>
  </view>
</template>

<script>
export default {
  data () {
    return {
      columnNum: 5,
      value: 2,
      label: '',
      list: [],
      radios: [3, 4, 5, 6, 7]
    }
  },
  created () {
    let list = []
    for (let i = 1; i < 20; i++) {
      list.push({
        label: `选项${i}`,
        value: i
      })
    }
    this.list = list
  },
  onReady () {
    this.$nextTick(() => {
      // 此处可以调用getColumnsInfo方法获取默认值的选项信息
      const info = this.$refs.picker1.getColumnsInfo(this.value)
      console.log('根据value获取的信息：', info)
      this.label = info.item.label
    })
  },
  methods: {
    handleTap (picker) {
      this.$refs[picker].show()
    },
    handleChange (e) {
      console.log('change::', e)
    },
    handleConfirm (e) {
      console.log('confirm::', e)
      if (e) {
        this.label = e.item.label
      }
    },
    handleCancel (e) {
      console.log('cancel::', e)
    },
    radioChange (e) {
      this.columnNum = +e.detail.value
    }
  }
}
</script>

<style lang="scss" scoped>
@import "../../index/index.scss";
</style>

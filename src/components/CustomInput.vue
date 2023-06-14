<template>
  <el-input v-model="defaultValue" @input="changeInput" v-bind="options" />
</template>
<script>
export default {
  name: 'CustomInput',
  props: {
    value: String,
    options: Object,
  },
  data() {
    return {
      defaultValue: '0',
    };
  },
  mounted() {
    this.defaultValue = this.transferToDecimal(this.value);
  },
  methods: {
    transferToDecimal(num) {
      if (!num) return;
      let [sInt, sFloat] = num.split('.');
      // 注意 “$&,”的应用
      sInt = sInt.replace(/\d(?=(\d{3})+$)/g, '$&,');
      // 区分整数与浮点小数
      return sFloat ? `${sInt}.${sFloat}` : `${sInt}`;
    },
    changeInput(value) {
      // 此处允许用户输入点 “.”, 同时如果再次输入点将会被截掉
      if (value.charAt(value.length - 1) == '.') {
        if (value.charAt(value.length - 2) == '.') {
          this.defaultValue = value.substr(0, value.length - 1);
        }
        return;
      }
      // 此处防止用户输入特殊字符，具体查看下面正则
      value = this.cleanInput(value);
      this.defaultValue = this.transferToDecimal(value);
      
      // 此处激活.sync默认行为实现props的双向绑定， 注意'update'
      this.$emit('update:value', value);
    },
    cleanInput(value) {
      // 正则表达式只匹配 首位负数， 数字， 以及第一个“.”号（如有多个只匹配一个），提取为数值默认顺序连接，然后通过转换函数转换为千分位
      return value.match(/(^-\d+|\d+|\.{1})/g, '').join('');
    },
  },
};
</script>

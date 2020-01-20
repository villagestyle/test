<template>
  <div class="home">
    <div>type属性</div>
    <self-button>默认按钮</self-button>
    <self-button type="danger">危险按钮</self-button>
    <self-button type="info">信息按钮</self-button>
    <self-button type="warn">警告按钮</self-button>
    <self-button type="primary">主要按钮</self-button>
    <div>size属性</div>
    <self-button type="danger" size="nomal">默认按钮</self-button>
    <self-button type="info" size="small">小按钮</self-button>
    <self-button type="default" size="large">大按钮</self-button>
    <self-button type="primary" size="mini">超小按钮</self-button>
    <div>color属性</div>
    <self-button color="grey">单色按钮</self-button>
    <self-button color="blue">单色按钮</self-button>
    <div>disabled</div>
    <self-button color="blue" :disabled="true">禁用按钮</self-button>
    <self-button type="primary" :disabled="true">禁用按钮</self-button>
    <div>round</div>
    <self-button type="primary" :round="true">圆角按钮</self-button>
    <self-button type="info" size="small" :round="true">小按钮</self-button>
    <div>round</div>
    <self-button type="primary" :round="true" @click="clickHandle">点击事件</self-button>
    <div>plain属性</div>
    <self-button type="default" :round="true" :plain="true">plain属性</self-button>
    <self-button type="primary" :round="true" :plain="true">plain属性</self-button>
    <self-button type="info" :round="true" :plain="true">plain属性</self-button>
    <self-button type="danger" :round="true" :plain="true">plain属性</self-button>
    <self-button type="warn" :round="true" :plain="true">plain属性</self-button>
  </div>
</template>

<script lang="ts">
import SelfButton from "../components/button/index.vue";
import { Vue, Component } from "vue-property-decorator";

@Component({
  components: { SelfButton }
})
export default class Home extends Vue {
  private clickHandle() {
    alert('11')
  }
}
</script>
<style lang="scss">
.home {
  div {
    margin-top: 35px;
    margin-bottom: 15px;
    text-align: left;
    border-bottom: 1px solid #797676;
  }
}
</style>

<template>
  <input :type="type" :placeholder="placeholder" v-model="value" />
</template>

<script lang="ts">
import Vue from "vue";
import { Component, Prop, Watch, Model } from "vue-property-decorator";
@Component({
  components: {}
})
export default class SelfInput extends Vue {
  @Prop({ default: "" }) placeholder?: string;
  @Prop({ default: "text" }) type?: string;

  @Model("input", { default: "" }) outData?: number | string;

  private value: number | string = "";

  @Watch("value")
  valueChange(newValue: any) {
    console.log("value值发生变化");
    this.value = newValue;
    this.$emit("input", this.value);
  }

  @Watch("outData")
  outDataCahnge(newValue: any) {
    console.log("外部值改变触发内部改变");
    this.value = newValue;
  }
}
</script>

<style lang="scss" scoped>
</style>
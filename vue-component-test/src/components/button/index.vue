<template>
  <button
    @click="clickHandle($event)"
    class="self-button"
    :class="classList"
    :style="styleList"
    :disabled="disabled"
  >
    <span>
      <slot></slot>
    </span>
  </button>
</template>

<script lang="ts">
import Vue from "vue";
import { Component, Prop } from "vue-property-decorator";

@Component({
  components: {}
})
export default class SelfButton extends Vue {
  @Prop({ default: "default" }) type?: string;
  @Prop({ default: false }) round?: boolean;
  @Prop({ default: false }) disabled?: boolean;
  @Prop({ default: "" }) color?: string;
  @Prop({ default: "default" }) size?: string;
  @Prop({ default: false }) plain?: boolean; // 线框风格

  private typeList: string[] = [
    "default",
    "primary",
    "info",
    "warn",
    "danger"
  ];
  private sizeList: string[] = ["large", "small", "mini", "normal"];

  private clickHandle(e: Event) {
    this.$emit("click", e);
  }

  get classList(): string[] {
    const arr: string[] = [];
    console.log(this.plain);
    if (this.plain) {
      arr.push("self-button--plain");
    }
    if (this.typeList.indexOf(this.type || "  ") === -1) {
      arr.push("self-button--default");
    } else {
      arr.push(`self-button--${this.type}`);
    }
    if (this.sizeList.indexOf(this.size || "  ") === -1) {
      arr.push("self-button--normal");
    } else {
      arr.push(`self-button--${this.size}`);
    }
    if (this.round) {
      arr.push("self-button--round");
    }
    console.log(this.disabled);
    if (this.disabled) {
      arr.push("state--disabled");
    }
    return arr;
  }

  get styleList(): any[] {
    const arr: any[] = [];
    if (this.color) {
      arr.push({ backgroundColor: `${this.color}` });
      arr.push({ color: `#fff` });
      arr.push({ borderColor: `${this.color}` });
    }
    console.log(arr);
    return arr;
  }
}
</script>

<style lang="scss" scoped>
@import "./index.scss";
@import "../../assets/scss/common.scss";
.self-button {
  border: none;
  outline: none;
  line-height: 42px;
  text-align: center;
  border-radius: 2px;
  cursor: pointer;
  position: relative;
}

.self-button::before {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 100%;
  height: 100%;
  background-color: #000;
  border: inherit;
  border-color: #000;
  border-radius: inherit;
  -webkit-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);
  opacity: 0;
  content: " ";
}

.self-button:active::before {
  opacity: 0.1;
}

.self-button--plain {
  &.self-button--primary {
    color: $color--primary;
    background-color: #fff;
    border: 1px solid $color--primary;
  }
  &.self-button--info {
    color: $color--info;
    background-color: #fff;
    border: 1px solid $color--info;
  }
  &.self-button--warn {
    color: $color--warning;
    background-color: #fff;
    border: 1px solid $color--warning;
  }
  &.self-button--danger {
    color: $color--danger;
    background-color: #fff;
    border: 1px solid $color--danger;
  }
}

.self-button--default {
  background-color: $color--default;
  border: 1px solid #ebedf0;
}

.self-button--primary {
  background-color: $color--primary;
  color: #fff;
}

.self-button--info {
  background-color: $color--info;
  color: #fff;
}

.self-button--warn {
  background-color: $color--warning;
  color: #fff;
}

.self-button--danger {
  background-color: $color--danger;
  color: #fff;
}

.self-button--round {
  border-radius: 200px;
}

.self-button--normal {
  padding: 0 15px;
  font-size: 14px;
}

.self-button--large {
  width: 100%;
  height: 50px;
  line-height: 48px;
}

.self-button--small {
  min-width: 60px;
  height: 30px;
  padding: 0 8px;
  font-size: 12px;
  line-height: 28px;
}

.self-button--mini {
  display: inline-block;
  min-width: 50px;
  height: 22px;
  font-size: 10px;
  line-height: 20px;
}

.state--disabled {
  opacity: 0.5;
  cursor: not-allowed;
}
</style>
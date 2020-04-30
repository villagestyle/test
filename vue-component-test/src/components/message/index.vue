<template>
  <div v-if="visible" class="self-message">
    <p>{{ title || '提示' }}</p>
    <div>{{ text }}</div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
const Message = {
  name: "message",
  props: ["visible", "text", "title", "onComfirm", "delay"],
  data() {
    return { time: 0, timer: null };
  },
  updated() {
    const ele: HTMLElement = document.querySelector(".self-message");
    if (!ele) {
      return;
    }
    this.time++;
    this.delay = Number(this.delay) ? this.delay : 2000;
    this.timer = setTimeout(() => {
      ele.style.top = "-56px";
      ele.style.opacity = "0";
      this.timer = setTimeout(() => {
        this.visible = false;
      }, 200);
    }, this.delay);
  },
  methods: {
    messageAndClose() {
      // this.onComfirm && this.onComfirm();
      // this.visible = false;
    },
    clear() {
      const ele: HTMLElement = document.querySelector(".self-message");
      if (!ele) {
        return;
      }
      clearTimeout(this.timer);
      ele.style.top = "-56px";
      ele.style.opacity = "0";
      this.visible = false;
    }
  }
};

export default Message;
// 使用单例模式
let instanceCache;
export const message = function(text, title, onComfirm = () => {}) {
  if (typeof title === "function") {
    onComfirm = title;
    title = undefined;
  }
  const ConfirmCtor = Vue.extend(Message);
  const getInstance = () => {
    if (!instanceCache) {
      instanceCache = new ConfirmCtor({
        propsData: {
          text,
          title,
          onComfirm
        }
      });
      // 生成dom
      instanceCache.$mount();
      document.body.appendChild(instanceCache.$el);
    } else {
      // 更新属性
      instanceCache.text = text;
      instanceCache.title = title;
      instanceCache.onComfirm = onComfirm;
    }
    return instanceCache;
  };

  const instance = getInstance();
  console.log(instance);
  // 确保更新的prop渲染到dom
  // 确保动画效果
  instance.clear();
  Vue.nextTick(() => {
    instance.visible = true;
    setTimeout(() => {
      instance.$el.style.top = "8px";
      instance.$el.style.opacity = "1";
    }, 200);
  });
};
</script>

<style lang="scss" scoped>
.self-message {
  border: 1px solid #eee;
  box-shadow: 1px 1px 1px 1px rgb(248, 248, 248);
  display: inline-block;
  padding: 8px;
  border-radius: 3px;
  position: fixed;
  top: -56px;
  left: 8px;
  font-size: 14px;
  transition: all 0.2s;
  opacity: 0;
  p {
    margin: 0;
    text-align: center;
    color: #ccc;
  }
}
</style>
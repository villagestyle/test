<template>
  <div id="self-toast" class="self-toast disabled" v-show="isShow">
    {{ context }}
  </div>
</template>

<script lang="ts">
import { Vue, Component, Prop } from "vue-property-decorator";

@Component
export default class Toast extends Vue {
  @Prop({ default: "" }) context!: string;
  @Prop({ default: 2 }) delay!: number;

  private isShow = false;
  private timer: number = 0;

  show() {
    clearTimeout(this.timer);
    this.isShow = true;
    const ele = document.querySelector("#self-toast");
    this.timer = window.setTimeout(() => {
      ele && ele.classList.add("disabled");
      this.timer = window.setTimeout(() => {
        this.isShow = false;
      }, 1000);
    }, this.delay * 1000);
  }
}

let instance: Toast;
export const toast = (data: { context: string; delay: number }) => {
  const getInstance = () => {
    if (!instance) {
      instance = new Toast({
        propsData: data
      });
      instance.$mount();
      document.body.appendChild(instance.$el);
    } else {
      instance.context = data.context;
      instance.delay = data.delay;
    }
    return instance;
  };

  instance = getInstance();
  Vue.nextTick(() => {
    instance.show();
    setTimeout(() => {
      instance.$el.classList.remove("disabled");
    }, 100);
  });
};
</script>

<style scoped lang="scss">
.self-toast {
  position: fixed;
  z-index: 99;
  top: 100px;
  left: 50%;
  transform: translateX(-50%);
  padding: 3px 5px;
  background-color: rgba(0, 0, 0, 0.4);
  transition: all 1s;
  border-radius: 3px;
  color: #fff;
  opacity: 1;
  &.disabled {
    opacity: 0;
  }
}
</style>

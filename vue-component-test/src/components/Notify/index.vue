<template>
  <div
    v-show="isShow"
    class="self-nitify"
    @mouseover="mouseOver"
    @mouseout="mouseOut"
  >
    {{ message }}
  </div>
</template>

<script lang="ts">
import { Vue, Component, Prop } from "vue-property-decorator";

@Component
export default class Notify extends Vue {
  @Prop({ default: "" }) message!: string;
  @Prop({ default: 2 }) delay!: number;
  @Prop({ default: () => {} }) mouseover!: Function;
  @Prop({ default: () => {} }) mouseout!: Function;

  public id = 0;
  private isShow = false;

  show() {
    this.isShow = true;
  }

  close() {
    this.isShow = false;
  }

  mouseOver() {
    this.mouseover();
  }

  mouseOut() {
    this.mouseout();
  }
}

let instances: Notify[] = [];
let messageId: number = 0;
export const notify = (data: { message: string; delay: number }) => {
  let timer = 0;

  const updateTop = () => {
    let h = 20;
    instances.map((instance, i) => {
      const ele = instance.$el as HTMLElement;
      h += ele.offsetHeight + 16;
      ele.style.top = h + "px";
      ele.style.transition = "all .3s";
    });
  };

  const curPosition = () => {
    let h = 20;
    instances.map((instance, i) => {
      const ele = instance.$el as HTMLElement;
      h += ele.offsetHeight + 16;
    });
    return h;
  };

  const close = () => {
    timer = window.setTimeout(() => {
      const curIndex = instances.findIndex(item => item.id === instance.id);
      instance.close();
      instances.splice(curIndex, 1);
      instance.$el.remove();
      updateTop();
    }, data.delay * 1000);
  };

  const mouseover = () => {
    clearTimeout(timer);
  };

  const mouseout = () => {
    close();
  };

  let instance: Notify;
  instance = new Notify({
    propsData: { ...data, mouseover, mouseout }
  });
  instance.id = messageId++;
  instances.push(instance);
  instance.$mount();
  document.body.appendChild(instance.$el);

  Vue.nextTick(() => {
    const ele = instance.$el as HTMLElement;
    ele["style"]["top"] = curPosition() + "px";
    instance.show();
    Vue.nextTick(() => {
      updateTop();
      close();
    });
  });
};
</script>

<style scoped>
.self-nitify {
  background-color: rgb(177, 177, 177);
  display: inline-block;
  padding: 10px 15px;
  border-radius: 3px;
  color: #edf2fc;
  position: fixed;
  left: 50%;
  transform: translateX(-50%);
}
</style>

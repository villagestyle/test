<template>
  <div class="treeList">
    <ul>
      <li v-for="(d,i) in data" :key="i">
        <span
          @click="nodeClick(d)"
          :class="{'openIcon':d.checked,'closeIcon':d.children&&d.children.length!=0,'active':!d.children&&d.lastgrada}"
        >{{d.text}}</span>
        <tree-list
          v-if="d.children&&d.children.length!=0&&d.checked"
          :treeData="d.children"
          @input="inputHandle"
          v-model="treeKey"
        ></tree-list>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue, Model, Watch } from "vue-property-decorator";
@Component
export default class TreeList extends Vue {
  @Prop({}) treeData?: any[];
  @Model("input") outData: any;

  private selectedTreeNode: any = {};
  private data: any = {};
  private treeKey: number | string = '';

  @Watch('outData')
  outDataChange() {
    console.log('外部改变了数据');
    console.log(this.outData);
  }

  mounted() {
    this.data = JSON.parse(JSON.stringify(this.treeData));
  }

  private nodeClick(d: any) {
    if (!d.children || d.children.length === 0) {
      d.checked = !d.checked;
      console.log('内部更新数值');
    }
    this.data.forEach(
      (tree: any) => (tree.lastgrada = d.id === tree.id && !tree.lastgrada)
    );
    this.$emit("input", d.id);
  }

  private inputHandle() {
    this.$emit("input", this.treeKey);
  }
}
</script>

<style lang="scss" scoped>
.openIcon {
  color: red;
}
.closeIcon {
  color: grey;
}
.active {
}
</style>
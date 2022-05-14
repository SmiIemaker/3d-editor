<template>
  <el-menu
    size="mini"
    mode="horizontal"
    @select="handleSelect"
    background-color="#545c64"
    active-text-color="#fdd440"
    text-color="#fff"
  >
    <el-submenu index="x">
      <template slot="title">新增</template>
      <el-submenu index="1-1">
        <template slot="title">geometry</template>
        <el-menu-item
          v-for="(item, idx) in geometryList"
          :key="item.name"
          :index="item.name"
        >
          <div
            class="tip"
            draggable="false"
            :src="item.img"
            data-type="geometry"
            :data-idx="idx"
            @click="addgeometry"
          >
            {{ item.name }}
          </div>
        </el-menu-item>
      </el-submenu>
      <!-- <el-submenu index="1-2">
        <template slot="title">bufferGeometry</template>
        <el-menu-item
          v-for="(item, idx) in bufferGeometryList"
          :key="item.name"
          :index="item.name"
        >
          <div
            class="tip"
            draggable="false"
            data-type="bufferGeometry"
            :data-idx="idx"
            @click="addbufferGeometry"
          >
            {{ item.name }}
          </div>
        </el-menu-item>
      </el-submenu> -->
      <el-submenu index="1-3">
        <template slot="title">light</template>
        <el-menu-item
          v-for="(item, idx) in lightList"
          :key="item.name"
          :index="item.name"
        >
          <div class="tip" draggable="false" :data-idx="idx" @click="addLight">
            {{ item.name }}
          </div>
        </el-menu-item>
      </el-submenu>
    </el-submenu>
  </el-menu>
</template>

<script lang="ts">
import { Component, Prop, Vue, Provide } from "vue-property-decorator";
import GameEvent from "@/core/event/index";

@Component({
  components: {},
})
export default class MenuView extends Vue {
  @Provide() activeNames: string = "0";

  @Provide() geometryList: Array<Object> = [];
  @Provide() bufferGeometryList: Array<Object> = [];
  @Provide() lightList: Array<Object> = [];

  mounted() {
    (<any>this).$axios.get("asset/data.json").then((res: any) => {
      this.geometryList = res.data.geometry;
      // this.bufferGeometryList = res.data.bufferGeometry;
      this.lightList = res.data.light;
    });

    this.$el.addEventListener("mousedown", (e: any) => {
      //e.preventDefault();
      let obj: any = e.target;
      console.log(obj);
      if (obj.className == "tip") {
        let item;
        console.log(item);
        if (obj.dataset.type == "bufferGeometry") {
          item = this.bufferGeometryList[obj.dataset.idx];
        } else {
          item = this.geometryList[obj.dataset.idx];
        }
        console.log(item, "-----item---");
        this.$store.commit("changeDrag", item);
        this.$store.commit("changeDragPosition", {
          x: e.clientX,
          y: e.clientY,
        });
        this.$store.commit("changeDragOffset", { x: e.offsetX, y: e.offsetY });

        window.addEventListener("mousemove", this.mouseMove);
        window.addEventListener("mouseup", this.mouseUp);
      }
    });
  }
  handleSelect(key: any, keyPath: any, e: MouseEvent) {
    console.log(key, keyPath);
  }
  handleCollapse(e: any) {
    console.log(e);
  }

  handleClick(e: any) {
    console.log(e);
    if (e.idx == "1") {
      GameEvent.ins.send(GameEvent.GET_SCENE_TREE, null);
    }
  }

  // handleNodeClick(e: any){
  //   console.log(e);
  //   GameEvent.ins.send(GameEvent.SELECT_TREE_ITEM, e.uuid);
  // }

  get sceneTree(): any {
    return this.$store.state.sceneTree;
  }

  mouseMove(e: MouseEvent) {
    // console.log("mouseMove " + this.tip);
    this.$store.commit("changeDragPosition", { x: e.clientX, y: e.clientY });
  }

  mouseUp(e: MouseEvent) {
    GameEvent.ins.send(GameEvent.DRAG_ITEM, null);
    window.removeEventListener("mousemove", this.mouseMove);
    window.removeEventListener("mouseup", this.mouseUp);
    this.$store.commit("changeDrag", null);
  }
  addgeometry(e: MouseEvent) {
    let obj: any = e.currentTarget;
    let item: any = this.geometryList[obj.dataset.idx];
    console.log(e);
    // this.$store.commit("changeDrag", item);
    console.log(item, "-----item---");
    this.$store.commit("changeDrag", item);
    this.$store.commit("changeDragPosition", {
      x: e.clientX,
      y: e.clientY,
    });
    this.$store.commit("changeDragOffset", { x: e.offsetX, y: e.offsetY });
    window.addEventListener("mousemove", this.mouseMove);
    window.addEventListener("mouseup", this.mouseUp);
  }
  // addbufferGeometry(e: MouseEvent) {
  //   let obj: any = e.currentTarget;
  //   let item: any = this.bufferGeometryList[obj.dataset.idx];
  //   console.log(e);
  //   // this.$store.commit("changeDrag", item);
  //   console.log(item, "-----item---");
  //   this.$store.commit("changeDrag", item);
  //   this.$store.commit("changeDragPosition", {
  //     x: e.clientX,
  //     y: e.clientY,
  //   });
  //   this.$store.commit("changeDragOffset", { x: e.offsetX, y: e.offsetY });
  //   window.addEventListener("mousemove", this.mouseMove);
  //   window.addEventListener("mouseup", this.mouseUp);
  // }
  addLight(e: MouseEvent) {
    let obj: any = e.currentTarget;
    let item: any = this.lightList[obj.dataset.idx];
    console.log(e);
    // this.$store.commit("changeDrag", item);
    GameEvent.ins.send(GameEvent.ADD_LIGHT, item.name);
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less" scoped>
@import "./index.less";
</style>

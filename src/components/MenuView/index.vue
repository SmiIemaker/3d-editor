<template>
    <div class="menu-view">
        <input ref="file" class="file" type="file" @change="handleSelectFile" />

        <el-menu
            size="mini"
            mode="horizontal"
            @select="handleSelect"
            background-color="#545c64"
            active-text-color="#fdd440"
            text-color="#fff"
        >
            <el-submenu index="1">
                <template slot="title"
                    >文件</template
                >
                <el-menu-item index="1-1">导入模型</el-menu-item>
                <el-submenu index="1-2">
                    <template slot="title"
                       >导出模型</template
                    >
                    <el-menu-item index="exp-0">导出object模型</el-menu-item>
                    <el-menu-item index="exp-1">导出stl模型</el-menu-item>
                    <el-menu-item index="gltf">导出gltf模型</el-menu-item>
                </el-submenu>
            </el-submenu>
        </el-menu>
        <AddView />
    </div>
</template>

<script>
import { Component, Prop, Vue, Provide } from "vue-property-decorator";
import AddView from "@/components/AddView/index.vue";
import GameEvent from "@/core/event/index";
import ParamTooler from "@/core/tool/ParamTooler";
import { mapState, mapMutations } from "vuex";
export default {
    data() {
        return {
            position: "",
            normal: "",
            uv: "",
            index: "",
            activeIndex: "1",
            activeIndex2: "1"
        };
    },
    components: {
        AddView,
    },
    computed: {
    },
    mounted() {},

    methods: {
        ...mapMutations(["changeVisible"]),
        handleSelect(key, keyPath) {
            console.log(key, keyPath);
            if (key == "1-1") {
                this.changeVisible({ key: "file", value: true });
                return;
            }
            if (key == "gltf") {
                this.changeVisible({ key: "gltf", value: true });
                return;
            }
            if (key.indexOf("exp") != -1) {
                GameEvent.ins.send(GameEvent.MODEL_EXPORT, {
                    type: key.split("-").pop(),
                    param: {}
                });
            }
        },
        handleSelectFile(e) {
            let f = e.target.files[0];
            if (f) {
                GameEvent.ins.send(GameEvent.IMPORT_SCENE, f);
            }
        },
    }
};
</script>

<style lang="less" scoped>
@import "./index.less";
</style>

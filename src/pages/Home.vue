<template>
    <div class="home">
        <Stage />
        <LeftSide />
        <RightSide />
        <TextureView />
        <MenuView />
        <FileView />
        <GltfConfig />
        <DragView />
    </div>
</template>

<script>
import { Component, Vue } from "vue-property-decorator";
import Stage from "@/components/Stage/index.vue";
import LeftSide from "@/components/LeftSide/index.vue";
import RightSide from "@/components/RightSide/index.vue";
import TextureView from "@/components/TextureView/index.vue";
import MenuView from "@/components/MenuView/index.vue";
import FileView from "@/components/FileView/index.vue";
import GltfConfig from "@/components/GltfConfig/index.vue";
import DragView from "@/components/DragView/index.vue";
import GameEvent from "@/core/event/index";

export default {
    data() {
        return {
            
        };
    },
    components: {
        Stage,
        LeftSide,
        RightSide,
        TextureView,
        MenuView,
        FileView,
        GltfConfig,
        DragView
    },
    mounted() {
        
    }
};
</script>

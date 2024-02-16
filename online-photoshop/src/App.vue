<template>
    <div class="main">
        <menu-component />
        <tools-component />
        <canvas-component ref="upload"/>
    </div>
</template>

<script>
import ToolsComponent from './components/ToolsComponent.vue';
import CanvasComponent from './components/CanvasComponent.vue';
import MenuComponent from './components/MenuComponent.vue';

export default {
  name: 'App',
  components: {
    MenuComponent, ToolsComponent, CanvasComponent
  }
}
</script>

<style lang="scss">
.main {
  width: 100%;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: flex-start;
  background-color: #DFDFDF;
}
</style>

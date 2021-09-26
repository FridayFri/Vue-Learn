<template>
  <teleport to="#modal">
    <div id="center" v-if="isOpen">
      <h1><slot> is a modal</slot></h1>
      <button @click="buttonClick">Colse</button>
    </div>
  </teleport>
</template>
<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "Modal",
  props: {
    isOpen: Boolean,
  },
  emits: {
    "close-modal": null,
  },
  setup(props, context) {
    const buttonClick = () => context.emit("close-modal");
    return{
        buttonClick
    }
  },
});
</script>
<style >
#center {
  width: 200px;
  height: 200px;
  border: 2px solid black;
  background: white;
  position: fixed;
  left: 50%;
  top: 50%;
  margin-left: -100px;
  margin-top: -100px;
}
</style>
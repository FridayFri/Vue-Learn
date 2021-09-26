<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <h1>{{ data.count }}</h1>
  <h1>{{ greetings }}</h1>
  <h1>{{ x }}</h1>
  <h1>{{ y }}</h1>
  <h1 v-if="loading">Loading</h1>
  <Moadl :isOpen="modalIsOpen" @close-modal="openModalClose">My Modal</Moadl>
  <button @click="openModal">Modal</button>
  <img v-if="loaded" :src="result.message" />
  <button @click="updateGreeting">+++</button>
</template>

<script lang="ts">
import { computed, onMounted, onUnmounted, reactive, ref, watch } from "vue";
import useMousePosition from "./hooks/useMousePosition";
import useURLLoader from "./hooks/useURLLoader";
import Moadl from "./components/Modal.vue";
interface DataProps {
  count: number;
  increase: () => void;
  double: number;
}
interface DogResult {
  message: string;
  status: string;
}

export default {
  name: "App",
  components: {
    Moadl,
  },
  setup() {
    const data: DataProps = reactive({
      count: 0,
      increase: () => {
        data.count++;
      },
      double: computed(() => data.count * 2),
    });
    const greetings = ref("");
    const updateGreeting = () => {
      greetings.value += "Hello!";
    };
    const { x, y } = useMousePosition();
    const { result, loading, loaded } = useURLLoader<DogResult>(
      "https://dog.ceo/api/breeds/image/random"
    );
    watch(greetings, (newValue, oldValue) => {
      document.title = greetings.value;
    });
    const modalIsOpen = ref(false);
    const openModal = () => {
      modalIsOpen.value = true;
    };
    const openModalClose = () => {
      modalIsOpen.value = false;
    };
    return {
      data,
      greetings,
      updateGreeting,
      x,
      y,
      result,
      loading,
      loaded,
      openModal,
      modalIsOpen,
      openModalClose,
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

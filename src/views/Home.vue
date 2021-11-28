<template>
  <div class="home flex flex-col justify-center items-center h-screen w-screen ">
    <h1 class="text-xl mb-24 text-center"> Colorie ton texte pour t'aider à bien prononcer !</h1>
    <tip-tap></tip-tap>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import TipTap from '../components/Tiptap.vue'

export default defineComponent({
  name: 'Home',
  components: {
    TipTap
  },
});
</script>

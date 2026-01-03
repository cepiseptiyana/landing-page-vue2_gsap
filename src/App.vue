<template>
  <div id="app">
    <Section1 />
    <h1>section 2</h1>
    <section class="container section-2"></section>
    <section class="container section-3"></section>
  </div>
</template>

<script>
import Section1 from "./components/Section1.vue";

export default {
  name: "App",
  components: { Section1 },
};
</script>

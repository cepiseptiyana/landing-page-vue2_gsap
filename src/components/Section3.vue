<template>
  <footer class="footer_component">
    <h2>{{ data }}</h2>
  </footer>
</template>

<script>
export default {
  name: "section-2",
  props: {
    msg: String,
  },
  data() {
    return {
      data: "Thank you",
    };
  },
};
</script>

<style lang="scss" scoped>
@import "@/assets/styles/variables";

.footer_component {
  background-color: $dark-color;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;

  & h2 {
    font-style: italic;
    margin-top: 0px;
    margin-bottom: 0px;
    font-family: "Recife Display", serif;
    font-size: 64px;
    line-height: 0.8311688311688312;
    font-weight: 400;
    letter-spacing: -0.02em;
  }
}
</style>

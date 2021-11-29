<template>
  <div class="container">
    <drop-box />
  </div>
</template>

<script>
import DropBox from "../components/DropBox";

export default {
  components: {DropBox}
}
</script>

<style scoped>
.container {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
}
</style>

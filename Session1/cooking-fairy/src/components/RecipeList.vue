<template>
  <div class="recipe-container">
    <ul>
      <li v-for="(step, index) in steps" :key="index">{{ step }}</li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    recipe: {
      type: String,
      required: true
    },
  },
  computed: {
    steps() {
      return this.recipe.split("\n").map((step) => step.trim());
    }
  }
}
</script>

<style scoped>
.recipe-container {
  text-align: left;
  padding: 20px;
  line-height: 1.5;
}
</style>
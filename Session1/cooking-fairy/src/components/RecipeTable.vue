<template>
  <div class="table-container">
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Ingredients</th>
          <th>Recipe</th>
        </tr>
      </thead>
      <tbody v-if="dishes.length>0">
        <tr v-for="(dish, index) in dishes" :key="index">
          <td>{{ dish.name }}</td>
          <td>
            <ul>
              <li v-for="(ingredient, index) in dish.ingredients" :key="index">{{ ingredient }}</li>
            </ul>
          </td>
          <td>
            <RecipeList :recipe="dish.recipe"></RecipeList>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import RecipeList from './RecipeList.vue';
export default {
  components: {
    RecipeList,
  },
  props: {
    dishes: {
      type: Array,
      required: true,
      default: () => [],
    },
  },
  updated() {
    console.log('dishes', this.dishes);
  }
}
</script>

<style scoped>
.table-container {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 30px;
}

table {
  border-collapse: collapse;
  width: 100%;
  max-width: 1000px;
  margin: 0 auto;
  border-radius: 10px;
  overflow: hidden;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
}

th,
td {
  padding: 12px 15px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}
</style>

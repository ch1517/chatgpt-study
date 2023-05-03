<template>
  <div>
    <h1>🍳👩‍🍳🔪</h1>
    <RecipeForm 
      :ingredients="ingredients"
      @setDishes="setDishes" 
      @setIngredients="setIngredients">
    </RecipeForm>
    <RecipeTable :dishes="dishes"></RecipeTable>
  </div>
</template>

<script>
import RecipeForm from './components/RecipeForm.vue';
import RecipeTable from './components/RecipeTable.vue';

export default {
  components: {
    RecipeForm,
    RecipeTable,
  },
  data() {
    return {
      ingredients: [], // 재료
      dishes: [], // 요리
    }
  },
  methods: {
    setIngredients(ingredients) {
      this.ingredients = ingredients;
    },
    setDishes(dishes) {
      console.log(dishes);
      const parsedDishes = JSON.parse(dishes);
      this.dishes = parsedDishes;
    }
  }
};
</script>
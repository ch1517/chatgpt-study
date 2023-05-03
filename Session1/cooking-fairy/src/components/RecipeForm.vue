<template>
  <div>
    <input v-model="newIngredient" type="text" placeholder="Input ingredient">
    <button @click="addIngredient">Add Recipe</button>
    <button @click="clearIngredients">Clear Recipe</button>
    <ul>
      <li v-for="(ingredient, index) in ingredients" :key="index">{{ ingredient }}</li>
    </ul>
  </div>
</template>

<script>
import { Configuration, OpenAIApi } from "openai";

const configuration = new Configuration({
  apiKey: process.env.VUE_APP_OPENAI_KEY,
});
export default {
  props : {
    ingredients: {
      type: Array,
      required: true,
      default: () => [],
    },
  },
  data() {
    return {
      newIngredient: '',
    }
  },
  methods: {
    getIngredientString() {
      return this.ingredients.join(', ');
    },
    addIngredient() {
      const ingredients = this.newIngredient.split(',').map(i => i.trim());
      this.$emit('setIngredients', [...this.ingredients, ...ingredients]);
      this.newIngredient = '';
      this.generateRecipe();
    },
    async generateRecipe() {
      const openai = new OpenAIApi(configuration);
      // https://community.openai.com/t/getting-response-data-as-a-fixed-consistent-json-response/28471/22?page=2
      const prompt = `Please tell me 3 dishes you can make with ${this.getIngredientString()}.\nDo not include any explanations, only provide a RFC8259 compliant JSON Array response following this format without deviation. [{"name": name of dish 1, "ingredients": Ingredients needed for dish 1 (string[] type), "recipe": description of dish 1 order}, (...cooks 2,3 same)]`;
      // const prompt = `Please tell me 2 dishes you can make with ${this.getIngredientString()}.\nPlease return the type as an array as follows [{"name": name of dish 1, "ingredients": Ingredients needed for dish 1 (string[] type), "recipe": description of dish 1 order}, (...cooks 2 same)]`;
      
      const response = await openai.createCompletion({
        model: "text-davinci-002",
        prompt: prompt,
        max_tokens: 1024,
        n: 1,
        stop: null,
        temperature: 0.7
      });
      const dishes = response.data.choices[0].text.trim();
      this.$emit('setDishes', dishes);
    },
    clearIngredients() {
      this.$emit('setIngredients', []);
    }
  },
}
</script>
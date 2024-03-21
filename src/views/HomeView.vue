<template>
  <div class="mx-10">

    <v-row class="justify-center mt-10">
      <div class="justify-center   d-flex mx-auto" style="width: 50%;">
        <v-text-field label="I'm looking for" v-model="searchValue" variant="outlined"></v-text-field>
        <v-btn :disabled="!searchValue" style="height: 52px; width: 150px  " class="ml-2" color="green"
          @click=" searchRecipesFn(), searchView = true, listView = false">Search</v-btn>
      </div>
    </v-row>

    <div v-if="listView">
      <v-row class="justify-center">
        <h2>Recipesd List</h2>
      </v-row>
      <v-row no-gutters class="justify-center">
        <v-col cols="4" v-for="(recipe, index) in recipes" :key="index">
          <v-card :loading="loading" outlined class="mx-5 my-10 ">
            <v-img height="170" :src="recipe.image"></v-img>
            <v-row class="justify-center text-center mx-5 mt-3">
              <h3>{{ recipe.name }}</h3>
              <v-divider class="mx-4 my-2"></v-divider>
              <p>{{ recipe.description }}</p>
              <v-divider class="mx-4 my-2"></v-divider>
              <p><b>Ingredients: </b>{{ recipe.ingredients.join(", ") }}</p>
              <v-divider class="mx-4 my-2"></v-divider>
              <p>Calories: {{ recipe.calories }}</p>
              <v-divider class="mx-4 my-5"></v-divider>
            </v-row>
          </v-card>
        </v-col>
      </v-row>
    </div>
  </div>


  <div v-if="searchView" class="mx-10">
    <v-row class="justify-center">
      <h2>Search List {{ searchRecipes.length }}</h2>
    </v-row>
    <v-row class="justify-center">
      <p style="color: red;"><b>{{ noResultsText }}</b></p>
    </v-row>


    <v-row class="mx-4">
      <v-col><v-btn @click="searchView = false, listView = true">Go Back</v-btn></v-col>
    </v-row>
    <v-row no-gutters class="justify-center">
      <v-col cols="4" v-for="(seaRecipes, index) in searchRecipes" :key="index">
        <v-card :loading="loading" outlined class="mx-5 my-10 ">
          <v-img height="170" :src="seaRecipes.image"></v-img>
          <v-row class="justify-center text-center mx-5 mt-3">
            <h3>{{ seaRecipes.name }}</h3>
            <v-divider class="mx-4 my-2"></v-divider>
            <p>{{ seaRecipes.description }}</p>
            <v-divider class="mx-4 my-2"></v-divider>
            <p><b>Ingredients: </b>{{ seaRecipes.ingredients.join(", ") }}</p>
            <v-divider class="mx-4 my-2"></v-divider>
            <p>Calories: {{ seaRecipes.calories }}</p>
            <v-divider class="mx-4 my-5"></v-divider>
          </v-row>
        </v-card>
      </v-col>
    </v-row>

  </div>

</template>


<script>
export default {
  name: 'HomeView',

  data() {

    return {
      searchRecipes: [],
      searchValue: [],
      searchView: false,
      listView: true,
      noResultsText: "",	
      recipes: [
        {
          name: "Classic Beef Lasagna",
          description: "Layers of tender pasta, savory beef ragu, creamy béchamel sauce, and melted cheese, baked to perfection.",
          image: "https://myfoodbook.com.au/sites/default/files/styles/card_c_xw_wp/public/recipe_photo/Classic_beef_lasagne.jpg",
          ingredients: ["lasagna noodles", "ground beef", "tomato sauce", "mozzarella cheese", "ricotta cheese", "parmesan cheese", "onions", "garlic"],
          calories: 500,
        },
        {
          name: "Lemon Herb Roast Chicken",
          description: "Juicy roast chicken infused with lemon and herbs, served with roasted vegetables for a satisfying meal.",
          image: "https://images.pexels.com/photos/376464/pexels-photo-376464.jpeg?auto=compress&cs=tinysrgb&w=600",
          ingredients: ["whole chicken", "lemon", "rosemary", "thyme", "garlic", "potatoes", "carrots", "olive oil"],
          calories: 450,
        },
        {
          name: "Vegetarian Chili",
          description: "Hearty and flavorful chili packed with beans, vegetables, and spices, perfect for a cozy night in.",
          image: "https://images.pexels.com/photos/958545/pexels-photo-958545.jpeg?auto=compress&cs=tinysrgb&w=600",
          ingredients: ["kidney beans", "black beans", "corn", "bell peppers", "onions", "tomatoes", "chili powder", "cumin", "garlic"],
          calories: 355,
        },
        {
          name: "Mushroom Risotto",
          description: "Creamy and indulgent risotto cooked with mushrooms, onions, garlic, and Parmesan cheese.",
          image: "https://images.pexels.com/photos/1267320/pexels-photo-1267320.jpeg?auto=compress&cs=tinysrgb&w=600",
          ingredients: ["Arborio rice", "mushrooms", "vegetable broth", "white wine", "onions", "garlic", "butter", "Parmesan cheese"],
          calories: 400,
        },
        {
          name: "Grilled Salmon with Asparagus",
          description: "Healthy and delicious grilled salmon fillets served with tender roasted asparagus spears.",
          image: "https://images.pexels.com/photos/566566/pexels-photo-566566.jpeg?auto=compress&cs=tinysrgb&w=600",
          ingredients: ["salmon fillets", "asparagus", "lemon", "olive oil", "salt", "pepper", "garlic"],
          calories: 350,
        },
        {
          name: "Caprese Salad",
          description: "A refreshing salad made with ripe tomatoes, fresh mozzarella, basil leaves, and balsamic glaze.",
          image: "https://images.pexels.com/photos/1482803/pexels-photo-1482803.jpeg?auto=compress&cs=tinysrgb&w=600",
          ingredients: ["tomatoes", "fresh mozzarella", "basil leaves", "balsamic glaze", "olive oil", "salt", "pepper"],
          calories: 250,
        },
        {
          name: "Chicken and Vegetable Stir-Fry",
          description: "Quick and easy stir-fry with tender chicken strips, colorful vegetables, and a savory sauce.",
          image: "https://images.pexels.com/photos/691114/pexels-photo-691114.jpeg?auto=compress&cs=tinysrgb&w=600",
          ingredients: ["chicken breast", "bell peppers", "broccoli", "carrots", "snow peas", "soy sauce", "ginger", "garlic", "rice"],
          calories: 380
        },
        {
          name: "Pesto Pasta",
          description: "Simple yet flavorful pasta tossed in homemade basil pesto sauce and garnished with pine nuts and Parmesan cheese.",
          image: "https://images.pexels.com/photos/769969/pexels-photo-769969.jpeg?auto=compress&cs=tinysrgb&w=600",
          ingredients: ["spaghetti", "basil pesto", "pine nuts", "Parmesan cheese", "olive oil", "garlic"],
          calories: 420
        },
        {
          name: "Taco Salad",
          description: "A vibrant salad with seasoned ground beef, lettuce, tomatoes, black beans, corn, avocado, and tortilla strips, drizzled with creamy cilantro dressing.",
          image: "https://images.pexels.com/photos/750073/pexels-photo-750073.jpeg?auto=compress&cs=tinysrgb&w=600",
          ingredients: ["ground beef", "lettuce", "tomatoes", "black beans", "corn", "avocado", "tortilla strips", "cilantro dressing", "cheddar cheese"],
          calories: 380
        },
        {
          name: "Vegetable Curry",
          description: "A fragrant and spicy curry dish loaded with assorted vegetables and simmered in a creamy coconut milk sauce.",
          image: "https://images.pexels.com/photos/1199960/pexels-photo-1199960.jpeg?auto=compress&cs=tinysrgb&w=600",
          ingredients: ["assorted vegetables (e.g., potatoes, carrots, bell peppers, peas)", "coconut milk", "curry paste", "onions", "garlic", "ginger", "rice"],
          calories: 320,
        },

        {
          name: "Caprese Salad",
          description: "A refreshing salad made with ripe tomatoes, fresh mozzarella, basil leaves, and balsamic glaze.",
          image: "https://images.pexels.com/photos/3026808/pexels-photo-3026808.jpeg?auto=compress&cs=tinysrgb&w=600",
          ingredients: ["tomatoes", "fresh mozzarella", "basil leaves", "balsamic glaze", "olive oil", "salt", "pepper"],
          calories: 250,
        },

      ],

    }
  },

  methods: {
    searchRecipesFn() {
      this.noResultsText = "";
      const searchTerms = this.searchValue.toLowerCase().split(/\s+/);
      this.searchRecipes = this.recipes.filter(recipe => {
        return searchTerms.some(term => {
          const ingredientsLower = recipe.ingredients.map(ingredient => ingredient.toLowerCase());
          return ingredientsLower.some(ingredient => ingredient.includes(term));
        });
      });
      this.showSearch = this.searchRecipes.length > 0;

      if (!this.showSearch) {
        this.noResultsText = "No recipes found for your search terms.";
      }
    }
  }
}




</script>

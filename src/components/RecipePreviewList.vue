<template>

  <b-container fluid>
    
    <v-if v-if="emptyResult"> No Results Found!</v-if>
          
    <v-if v-if="title == 'Random Recipes' || title == 'Last Watched Recipes'">
        <b-row>
            <b-col v-for="r in recipes" :key="r.id" cols="6">
            <RecipePreview class="recipePreview" :recipe="r"></RecipePreview>
            </b-col>
        </b-row>
    </v-if>

    <v-else v-else>
        <b-row>
            <b-col v-for="r in recipes" :key="r.id" cols="3">
              <RecipePreview class="recipePreview" :recipe="r"></RecipePreview>
            </b-col>
        </b-row>
    </v-else>

  </b-container>
    
</template>
  
  
<!-- #######################################################################################################
############################################ scripts ################################################## -->
  
<script>

  import RecipePreview from "./RecipePreview.vue";
  export default {
    name: "RecipePreviewList",
    components: {
      RecipePreview
    },
    
    props: {
      title: {
        type: String,
        required: true
      }
    },
  
    data() {
      return {
        emptyResult:false,
        recipes:[ //TODO delete this temp data
                  {
                    id: 641726,
                    title: "Dulce De Leche Brownies",
                    readyInMinutes: 45,
                    image: "https://spoonacular.com/recipeImages/641726-556x370.jpg",
                    aggregateLikes: 29,
                    vegan: true,
                    vegetarian: true,
                    glutenFree: true
                  },
                  {
                    id: 651389,
                    title: "Medenjaci - Croatian Honey Spice Cookies",
                    readyInMinutes: 45,
                    image: "https://spoonacular.com/recipeImages/651389-556x370.jpg",
                    aggregateLikes: 6,
                    vegan: true,
                    vegetarian: false,
                    glutenFree: false
                  },
                  {
                    id: 425578,
                    title: "meast",
                    readyInMinutes: 35,
                    image: "https://spoonacular.com/recipeImages/641726-556x370.jpg",
                    aggregateLikes: 99,
                    vegan: true,
                    vegetarian: true,
                    glutenFree: false
                  }
                ]
      };
    },
  
    mounted() { //TODO uncomment this when using the server
      // if (this.title == 'Random Recipes'){
      //   this.getRandomRecipes();
      // }

      // if (this.title == 'Last Watched Recipes'){
      //   this.getLastWatchedRecipes();
      // }

      // if (this.title == 'Favorite Recipes'){
      //   this.getFavoriteRecipes();
      // }

      // if (this.title == 'My Recipes'){
      //   this.getMyRecipes();
      // }

      // if (this.title == 'Family Recipes'){
      //   this.getFamilyRecipes();
      // }
    },
  
    methods: {
      async getRandomRecipes() {
        try {
          const response = await this.axios.post(
            this.$root.store.server_domain + "/recipes/randomRecipes",
            { withCredentials: true }
          );

          this.getRecipesList(response);
        } catch (error) {
          console.log(error);
        }
      },

      async getLastWatchedRecipes(){
        try {
          const response = await this.axios.get(
            this.$root.store.server_domain + "/users/lastWatchedRecipes",
            { withCredentials: true }
          );

          this.getRecipesList(response);
        } catch (error) {
          console.log(error);
        }
      },

      async getFavoriteRecipes(){
        try {
          const response = await this.axios.get(
            this.$root.store.server_domain + "/users/favorites",
            { withCredentials: true }
          );

          this.getRecipesList(response);
        } catch (error) {
          console.log(error);
        }
      },

      async getMyRecipes(){
        try {
          const response = await this.axios.get(
            this.$root.store.server_domain + "/users/myAllRecipes",
            { withCredentials: true }
          );

          this.getRecipesList(response);
        } catch (error) {
          console.log(error);
        }
      },

      async getFamilyRecipes(){
        try {
          const response = await this.axios.get(
            this.$root.store.server_domain + "/users/familyRecipes",
            { withCredentials: true }
          );

          this.getRecipesList(response);
        } catch (error) {
          console.log(error);number
        }
      },

      async searchQuery(query, number, cuisine, diet, intolerance, sortFilter){
        this.emptyResult = false
        this.recipes = []
        let temp = []
        if(cuisine == "ALL"){
          cuisine = undefined
        }

        if(diet == "None"){
          diet = undefined
        }

        if(intolerance == "None"){
          intolerance = undefined
        }
        
        try {
          const response = await this.axios.post(
            "/recipes/search",
            {params:{query:query, number:number, cuisine:cuisine, diet:diet, intolerance:intolerance}},
          );

          if (response.data.length != 0){
            for (let i = 0; i < response.data.length; i++){
              temp.push(response.data[i])
            }
          }

          else {
            this.emptyResult = true
          }

          if(sortFilter == "Default"){
          }

          else if(sortFilter == "Prepare Time"){
            temp.sort((a, b) => parseInt(a.readyInMinutes) - parseInt(b.readyInMinutes));
          }

          else{ // Stars
            temp.sort((a, b) => parseFloat(a.popularity) - parseFloat(b.popularity));
          }
          
          this.recipes = temp
          return

        } catch (error) {
          console.log(error);
        }
      },

      async getRecipesList(response){
        const recipes = response.data;
        for (let i = 0; i < recipes.length; i++){
          try {
            const check = await fetch(recipes[i].image);
            if (!check.ok){
              recipes[i].image="https://img.freepik.com/free-vector/404-error-background-with-orange-piece-flat-style_23-2147761281.jpg"
            }
          } catch (error) {
            recipes[i].image="https://img.freepik.com/free-vector/404-error-background-with-orange-piece-flat-style_23-2147761281.jpg"
          }
        }
          
        this.recipes = [];
        this.recipes.push(...recipes);
      }
    }
  };
  
</script>
  
  
<!-- #######################################################################################################
################################################# css ################################################## -->
  
<style lang="scss" scoped>
  
  .container {
    min-height: 400px;
  }
  
</style>
  
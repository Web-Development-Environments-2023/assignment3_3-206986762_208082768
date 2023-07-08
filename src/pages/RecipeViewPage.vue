<template>
  <div class="container">

    <div id="background"></div>

    <div v-if="recipe">
      <table width="100%"> 
        <tr>

          <td width="55%">
            <h1 style="text-align: center; font-weight: bolder;font-size: 45px; font-family:Georgia, 'Times New Roman', Times, serif;">{{ recipe.title }}</h1>
            <br><br>
            <h2><span style="font-weight: bold;">Total Time:</span> <span style="color: red; font-weight: bold;">{{ recipe.readyInMinutes }}</span> minutes</h2>
            <h2><span style="color: red; font-weight: bold;">{{ recipe.aggregateLikes }}</span> likes</h2> <br><br>
            <div class="centered-list">
              <ul>
                <li>
                  <span :style="recipe.vegetarian ? 'color: green' : 'color: red'">
                    {{ recipe.vegetarian ? 'Vegetarian' : 'Not vegetarian' }}
                  </span>
                </li>

                <li>
                  <span :style="recipe.vegan ? 'color: purple;' : 'color: red;'">
                    {{ recipe.vegan ? 'vegan' : 'Not vegan' }}
                  </span><br>
                </li>

                <li>
                  <span :style="recipe.glutenFree ? 'color: blue' : 'color: red;'">
                    {{ recipe.glutenFree ? 'No gluten' : 'With gluten' }}
                  </span>
                </li>
              </ul>
            </div>
          </td>
          
          <td width="45%">
            <img :src="recipe.image" class="center" />
          </td>

        </tr>

        <tr>
          <td colspan="2">
            <br><br>
            <hr>
            <br>
          </td>
        </tr>

        <tr>
          <td colspan="2">
            <div class="recipe-body">

              <div class="wrapper">
                <div class="wrapped">
                  <h3><u>Ingredients:</u></h3>
                  <ul>
                    <li
                      v-for="(r, index) in recipe.extendedIngredients"
                      :key="index + '_' + r.id">
                      <h4 style="font-size:larger;">{{ r.original }}</h4>
                    </li>
                  </ul>
                </div>

                <div class="wrapped">
                  <h3><u>Instructions:</u></h3>
                  <ol>
                    <li v-for="s in recipe.instructions" :key="s.number">
                      <h4 style="font-size:larger;">{{ s.step }}</h4>
                    </li>
                  </ol>
                </div>
              </div>
            </div>
          </td>
        </tr>

      </table>
    </div>

  </div>
</template>


<!-- #######################################################################################################
############################################ scripts ################################################## -->

<script>

  export default {
    data() {
      return { //TODO recipe: null
        recipe:
                {
                  id: 641726,
                  title: "Dulce De Leche Brownies",
                  readyInMinutes: 45,
                  image: "https://spoonacular.com/recipeImages/641726-556x370.jpg",
                  aggregateLikes: 29,
                  popularity: 5,
                  vegan: true,
                  vegetarian: true,
                  glutenFree: true,
                  instructions: {},
                  servings: 4,
                  ingredients: {}
                }
        
      };
    },
    
    async created() { //TODO uncomment this when working with server
      try {
        let response;

        try { 
          response = await this.axios.get(
            this.$root.store.server_domain + "/recipes/" + this.$route.params.recipeId,
            { withCredentials: true }
          );

          if (response.status !== 200) this.$router.replace("/NotFound");
        } catch (error) {
          console.log("error.response.status", error.response.status);
          this.$router.replace("/NotFound");
          return;
        }

        let {
          analyzedInstructions,
          instructions,
          extendedIngredients,
          aggregateLikes,
          readyInMinutes,
          image,
          title
        } = response.data.recipe;

        let _instructions = analyzedInstructions
          .map((fstep) => {
            fstep.steps[0].step = fstep.name + fstep.steps[0].step;
            return fstep.steps;
          })
          .reduce((a, b) => [...a, ...b], []);

        let _recipe = {
          instructions,
          _instructions,
          analyzedInstructions,
          extendedIngredients,
          aggregateLikes,
          readyInMinutes,
          image,
          title
        };

        this.recipe = _recipe;
      } catch (error) {
        console.log(error);
      }
    }
  };

</script>


<!-- #######################################################################################################
################################################# css ################################################## -->

<style scoped>


  /* table, th, td {
    border: 1px solid black;
    padding: 8px;
    text-align: left;
  } */

  .container{
    margin-top: 80px;
    max-width: 100%;  
  }

  #background {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-repeat: no-repeat;
    margin: 0;
    background-image: url("../assets/RecipeView.jpg");
    background-size: cover;
    background-position: center;
    z-index: -1;
    opacity: 0.9;
  }

  h2 {
    text-align: center;
    font-size:x-large;
  }

  .centered-list {
    text-align: center;
    font-size:x-large;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .centered-list ul {
    list-style:disc;
    text-align: center;
  }

  .centered-list li {
    margin-bottom: 10px;
  }

  hr {
    border: none;
    border-top: 4px solid;
    font-weight: bold;
  }

  h3 {
    font-size:x-large;
    color: red;
    font-weight: bold;
  }

  .wrapper {
    display: flex;
  }

  .wrapped {
    width: 50%;
  }

  .center {
    display: block;
    margin-left: auto;
    margin-right: auto;
    width: 100%;
  }

</style>
<template>
  <div>

    <b-card 
      :title = "recipe.title"
      sub-title="Recipe"
      :img-src="recipe.image"
      img-alt="Image"
      img-top
      style="max-width: 20rem;"
      class="mb-2"><br>
      
      <b-card-text>
        {{ recipe.readyInMinutes }} minutes  |  {{ recipe.aggregateLikes }} likes <br>
        {{ recipe.vegetarian ? 'vegetarian' : 'Not vegetarian' }}  |  {{ recipe.vegan ? 'vegan' : 'Not vegan' }}  |  {{ recipe.glutenFree ? 'No gluten' : 'With gluten' }}
      </b-card-text>
      
      <b-button @click="toggleFavorite" variant="link" size="sm" class="favorite-button">
        <b-icon :icon="isFavorite ? 'heart-fill' : 'heart'" class="favorite-icon" />
      </b-button> <br><br>
    
      <router-link :to="{ name: 'recipe', params: { recipeId: recipe.id } }" class="recipe-preview">
        <b-button variant="primary">Learn More!</b-button>
      </router-link>

    </b-card>

  </div>
</template>


<!-- #######################################################################################################
############################################ scripts ################################################## -->

<script>
  export default {
    mounted() {
    },

    data() {
      return {
        isFavorite: false
      };
    },

    props: {
      recipe: {
        type: Object,
        required: true
      }
    },

    methods: {
      async toggleFavorite() { //TODO need to check if works!
        if (this.$root.store.username) {

          this.isFavorite = !this.isFavorite;
          if (this.isFavorite) {
            try {
              const response = await this.axios.post(
                this.$root.store.server_domain + "/users/favorites",
                {recipeId: id},
                { withCredentials: true }
              );

              if (response.status === 200){
                this.$root.toast("Add to favorites", "Recipe was added to favorites successfully!", "success");
              }
            } catch(error){
              this.$root.toast("Add to favorites", "Recipe was not added!", "danger");
            }
          }
        }

        else {
          this.$root.toast("Add to favorites", "You must login to add favorites!", "danger");
        }     
      }


      //TODO need to add another handler for the watched recipes (like favorites)
    },

  };

</script>


<!-- #######################################################################################################
################################################# css ################################################## -->

<style scoped>

  .favorite-icon {
    font-size: 1.5rem;
    color: red;
  }

  .recipe-preview {
    display: inline-block;
    width: 90%;
    height: 100%;
    position: relative;
    margin: 10px 10px;
  }

  .recipe-preview > .recipe-body {
    width: 100%;
    height: 200px;
    position: relative;
  }

  .recipe-preview .recipe-body .recipe-image {
    margin-left: auto;
    margin-right: auto;
    margin-top: auto;
    margin-bottom: auto;
    display: block;
    width: 98%;
    height: auto;
    -webkit-background-size: cover;
    -moz-background-size: cover;
    background-size: cover;
  }

  .recipe-preview .recipe-footer {
    width: 100%;
    height: 50%;
    overflow: hidden;
  }

  .recipe-preview .recipe-footer .recipe-title {
    padding: 10px 10px;
    width: 100%;
    font-size: 12pt;
    text-align: left;
    white-space: nowrap;
    overflow: hidden;
    -o-text-overflow: ellipsis;
    text-overflow: ellipsis;
  }

  .recipe-preview .recipe-footer ul.recipe-overview {
    padding: 5px 10px;
    width: 100%;
    display: -webkit-box;
    display: -moz-box;
    display: -webkit-flex;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-flex: 1;
    -moz-box-flex: 1;
    -o-box-flex: 1;
    -webkit-flex: 1 auto;
    -ms-flex: 1 auto;
    flex: 1 auto;
    table-layout: fixed;
    margin-bottom: 0px;
  }

  .recipe-preview .recipe-footer ul.recipe-overview li {
    -webkit-box-flex: 1;
    -moz-box-flex: 1;
    -o-box-flex: 1;
    -ms-box-flex: 1;
    -webkit-flex-grow: 1;
    flex-grow: 1;
    width: 90px;
    display: table-cell;
    text-align: center;
  }

</style>

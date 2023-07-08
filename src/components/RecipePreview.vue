<template>
  <div>
    <b-card no-body style="max-width: 20rem">
      <a
        style="cursor: pointer"
        class="card-image-link"
        @click="handleClick(recipe.id)"
      >
        <b-card-img :src="recipe.image" alt="Image"></b-card-img>
      </a>

      <b-card-body>
        <b-card-title :style="getTitleStyle">{{ recipe.title }}</b-card-title>
        <b-card-sub-title>Recipe</b-card-sub-title><br />

        <b-card-text>
          <span style="color: red; font-weight: bold">{{
            recipe.readyInMinutes
          }}</span>
          minutes |
          <span style="color: red; font-weight: bold">{{
            recipe.aggregateLikes
          }}</span>
          likes <br /><br />
          <span :style="recipe.vegetarian ? 'color: green' : 'color: red'">
            {{ recipe.vegetarian ? "Vegetarian" : "Not vegetarian" }}
          </span>
          |

          <span :style="recipe.vegan ? 'color: purple;' : 'color: red;'">
            {{ recipe.vegan ? "vegan" : "Not vegan" }}
          </span>
          |

          <span :style="recipe.glutenFree ? 'color: blue' : 'color: red;'">
            {{ recipe.glutenFree ? "No gluten" : "With gluten" }}
          </span>

          <!-- <span v-if="getFamily">
            {{ recipe.owner }}
          </span>
          <span v-if="getFamily">
            {{ recipe.whenToPrepare }}
          </span> -->
        </b-card-text>

        <b-button
          @click="toggleFavorite(recipe.id)"
          variant="link"
          size="sm"
          class="favorite-button"
        >
          <b-icon
            :icon="isFavorite ? 'heart-fill' : 'heart'"
            class="favorite-icon"
          />
        </b-button>
        <br /><br />

        <router-link
          :to="{ name: 'recipe', params: { recipeId: recipe.id } }"
          class="recipe-preview"
        >
          <b-button variant="primary">Learn More!</b-button>
        </router-link>
      </b-card-body>
    </b-card>
  </div>
</template>


<!-- #######################################################################################################
############################################ scripts ################################################## -->

<script>
export default {
  data() {
    return {
      isViewed: false,
      isFavorite: false
      // isFamily: false,
    };
  },

  created() {
    this.isViewed =
      localStorage.getItem(`viewedState:${this.recipe.id}`) === "true";
    this.isFavorite =
      localStorage.getItem(`favoriteState:${this.recipe.id}`) === "true";
  },

  computed: {
    getTitleStyle() {
      return {
        color: this.isViewed ? "rgb(38, 0, 255)" : "black",
      };
    },
  },

  props: {
    recipe: {
      type: Object,
      required: true,
    },
  },

  methods: {
    handleClick(recipeID) {
      this.navigateToRecipe(recipeID);
      this.toggleViewed(recipeID);
    },

    async navigateToRecipe(recipeId) {
      this.$router.push({ name: "recipe", params: { recipeId } });
    },

    async toggleFavorite(recipeID) {
      //TODO need to check if works!
      if (this.$root.store.username) {
        this.isFavorite = !this.isFavorite;
        this.$emit("toggle-favorite", recipeID, this.isFavorite);
        localStorage.setItem(
          `favoriteState:${this.recipe.id}`,
          this.isFavorite
        );

        if (this.isFavorite) {
          try {
            const response = await this.axios.post(
              this.$root.store.server_domain + "/users/favorites",
              { recipeId: id },
              { withCredentials: true }
            );

            if (response.status === 200) {
              this.$root.toast(
                "Add to favorites",
                "Recipe was added to favorites successfully!",
                "success"
              );
            }
          } catch (error) {
            this.$root.toast(
              "Add to favorites",
              "Recipe was not added!",
              "danger"
            );
          }
        }
      } else {
        this.$root.toast(
          "Add to favorites",
          "You must login to add favorites!",
          "danger"
        );
      }
    },

    // async toggleFamily() {
    //   console.log("Hi");
    //   //TODO need to check if works!
    //   if (this.$root.store.username) {
    //     this.$emit("toggle-family", recipeID, this.isFamily);
    //     // TODO
    //   } else {
    //     this.$root.toast(
    //       "You must login to add favorites!",
    //       "danger"
    //     );
    //   }
    // },

    async toggleViewed(recipeID) {
      //TODO need to check if works! and to add post for whatched recipes
      if (this.$root.store.username) {
        if (!this.isViewed) {
          this.isViewed = !this.isViewed;
          this.$emit("toggle-viewed", recipeID, this.isViewed);
          localStorage.setItem(`viewedState:${this.recipe.id}`, this.isViewed);
        } else {
          try {
            const response = await this.axios.post(
              this.$root.store.server_domain + "/users/favorites",
              { recipeId: id },
              { withCredentials: true }
            );

            if (response.status === 200) {
              this.$root.toast(
                "Add to watch list",
                "Recipe was added to watch list successfully!",
                "success"
              );
            }
          } catch (error) {
            this.$root.toast(
              "Add to watch list",
              "Recipe was not added!",
              "danger"
            );
          }
        }
      } else {
        this.$root.toast(
          "Add to watch list",
          "You must login to add watched recipes!",
          "danger"
        );
      }
    },

    updateIsViewed_IsFavorite(flag) {
      this.isViewed = flag;
      this.isFavorite = flag;
    },
  },
};
</script>


<!-- #######################################################################################################
################################################# css ################################################## -->

<style scoped>
.card-image-link {
  display: block;
  width: 100%;
  height: 100%;
  text-decoration: none;
  color: inherit;
}

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

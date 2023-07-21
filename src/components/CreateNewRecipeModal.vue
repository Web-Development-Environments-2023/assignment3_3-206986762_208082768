<template>
  <div class="container">
    <div class="modal-container">
      <b-modal
        id="modal-prevent-closing"
        ref="modal"
        title="Create New Recipe"
        @show="resetModal"
        @hidden="resetModal"
        @ok="handleOk"
      >
        <form ref="form" @submit.stop.prevent="handleSubmit">
          <b-form-group
            label="Title"
            label-for="title-input"
            invalid-feedback="Title is required"
            :state="titleState"
          >
            <b-form-input
              id="title-input"
              v-model="title"
              :state="titleState"
              required
            ></b-form-input>
          </b-form-group>

          <b-form-group
            label="Ready in Minutes"
            label-for="minutes-input"
            invalid-feedback="Ready in Minutes is required"
            :state="minutesState"
          >
            <b-form-input
              id="minutes-input"
              v-model="readyInMinutes"
              :state="minutesState"
              type="number"
              required
            ></b-form-input>
          </b-form-group>

          <div class="form-row">
            <div class="col-sm-4">
              <b-form-group label="Vegetarian">
                <b-form-checkbox v-model="vegetarian" name="vegetarian"
                  >Vegetarian</b-form-checkbox
                >
              </b-form-group>
            </div>

            <div class="col-sm-4">
              <b-form-group label="Vegan">
                <b-form-checkbox v-model="vegan" name="vegan"
                  >Vegan</b-form-checkbox
                >
              </b-form-group>
            </div>

            <div class="col-sm-4">
              <b-form-group label="Gluten Free">
                <b-form-checkbox v-model="glutenFree" name="glutenFree"
                  >Gluten Free</b-form-checkbox
                >
              </b-form-group>
            </div>
          </div>

          <b-form-group
            label="Servings"
            label-for="servings-input"
            invalid-feedback="Servings is required"
            :state="servingsState"
          >
            <b-form-input
              id="servings-input"
              v-model="servings"
              :state="servingsState"
              type="number"
              required
            ></b-form-input>
          </b-form-group>

          <b-form-group
            label="Instructions"
            label-for="instructions-textarea"
            invalid-feedback="Instructions are required"
            :state="instructionsState"
          >
            <b-form-textarea
              id="instructions-textarea"
              v-model="instructions"
              :state="instructionsState"
              name="instructions"
              required
            ></b-form-textarea>
          </b-form-group>

          <b-form-group
            label="Ingredients (comma-separated)"
            label-for="ingredients-input"
            invalid-feedback="Ingredients are required"
            :state="ingredientsState"
          >
            <b-form-input
              id="ingredients-input"
              v-model="ingredients"
              :state="ingredientsState"
              name="ingredients"
              required
            ></b-form-input>
          </b-form-group>

          <b-button type="submit" variant="primary">Submit</b-button>
        </form>
      </b-modal>
    </div>
  </div>
</template>

<script>
export default {
  name: "CreateNewRecipeModal",
  data() {
    return {
      title: "",
      readyInMinutes: null,
      vegetarian: false,
      vegan: false,
      glutenFree: false,
      servings: null,
      instructions: "",
      ingredients: "",
      titleState: null,
      minutesState: null,
      servingsState: null,
      instructionsState: null,
      ingredientsState: null,
    };
  },
  methods: {
    checkFormValidity() {
      const valid = this.$refs.form.checkValidity();
      this.titleState = valid;
      this.minutesState = valid;
      this.servingsState = valid;
      this.instructionsState = valid;
      this.ingredientsState = valid;
      return valid;
    },
    resetModal() {
      this.title = "";
      this.readyInMinutes = null;
      this.vegetarian = false;
      this.vegan = false;
      this.glutenFree = false;
      this.servings = null;
      this.instructions = "";
      this.ingredients = "";
      this.titleState = null;
      this.minutesState = null;
      this.servingsState = null;
      this.instructionsState = null;
      this.ingredientsState = null;
    },
    handleOk(bvModalEvent) {
      bvModalEvent.preventDefault();
      this.handleSubmit();
    },
    handleSubmit() {
      if (!this.checkFormValidity()) {
        return;
      }

      // Parse comma-separated ingredients into JSON array
      const ingredientsArray = this.ingredients
        .split(",")
        .map((ingredient) => ingredient.trim());

      // Create a recipe object with the form data
      const recipe = {
        title: this.title,
        readyInMinutes: this.readyInMinutes,
        vegetarian: this.vegetarian,
        vegan: this.vegan,
        glutenFree: this.glutenFree,
        servings: this.servings,
        instructions: this.instructions,
        ingredients: ingredientsArray,
      };

      // Perform additional logic, such as saving the recipe or performing other actions
      console.log("Recipe:", recipe);

      // Reset the form and hide the modal
      this.resetModal();
      this.$nextTick(() => {
        this.$bvModal.hide("modal-prevent-closing");
      });
    },
  },
};
</script>

<style>

.modal-container {
  position: absolute;
  top: 100px; 
  left: 0;
  right: 0;
  margin: 0 auto;
  max-width: 600px; 
}
#modal-prevent-closing {
  margin-top: 80px;
  max-width: 100%;
}
</style>

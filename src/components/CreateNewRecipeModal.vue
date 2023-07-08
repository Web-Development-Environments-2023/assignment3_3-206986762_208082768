<template>
    <div class="container">

        <b-modal id="myModal" title="Modal Title">
            <p>Modal content goes here</p>
        </b-modal>

    </div>
</template>
  
  
<!-- #######################################################################################################
############################################ scripts ################################################## -->

<script>

    export default {
        name: "CreateNewRecipeModal",
        data() {
            return {
                showModal: false,
            };
        },

    };

</script>
  
  
<!-- #######################################################################################################
################################################# css ################################################## -->

<style lang="scss">

    #myModal{
        margin-top: 80px;
        max-width: 100%;  
    }

</style>
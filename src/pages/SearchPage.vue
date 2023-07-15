<template>
  <div class="container">
    <div id="background"></div>
    
  </div>
</template>


<!-- #######################################################################################################
############################################ scripts ################################################## -->

<script>

  export default {
    name: "SearchPage"
  };

</script>


<!-- #######################################################################################################
################################################# css ################################################## -->

<style lang="scss">

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
background-image: url("../assets/background.jpg");
background-size: cover;
background-position: center;
z-index: -1;
opacity: 0.9;
}

</style>
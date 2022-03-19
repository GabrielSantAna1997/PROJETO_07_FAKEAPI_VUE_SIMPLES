<template>
  <div id="main-banner">
    <h1>Monte seu lanche</h1>
  </div>
</template>

<script>
  export default {
    name: "Banner"
  }
</script>

<style scoped>

  #main-banner {
    background-image: url('/img/bulguer.jpg');
    background-position: center;
    background-size: cover;
    height: 500px;
    display: flex;
    align-items: center;
    justify-content: flex-start;
    background-repeat: no-repeat;
  }

  #main-banner h1 {
    color: #FFF;
    text-align: center;
    font-size: 37px;
    background-color: black;
    padding: 20px 40px;
  }

</style>
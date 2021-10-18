<template>
<div id="nav">
  <router-link to="/">Home</router-link>
  <router-link to="/pedidos">Pedidos</router-link>
</div>
</template>

<script>

export default {
  name: "Navbar"
}
</script>

<style scoped>
#nav{
  background-color: rgb(0, 0, 0);
  justify-content: flex-end;
  padding: 10px;
  border-bottom: 2px solid rgb(0, 0, 0);
  display: flex;
}


#nav a {
  color: white;
  text-decoration: none;
  margin: 15px;
}

#nav a:hover {
  color: rgb(8, 255, 173);
}

</style>
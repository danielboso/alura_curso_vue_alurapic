<template>
  <ul class="navbar-nav mr-auto">  
    <li class="nav-item" v-for="rota in rotas">
      <router-link :to="rota.path ? rota.path : '/'" class="nav-link">
        {{ rota.titulo }}
      </router-link>
    </li>
  </ul>    
</template>

<script>

export default {

    props: {

        rotas: {
            type: Array,
            required: true
        }
    }
}

</script>

<style>
</style>
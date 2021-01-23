<template>
  <div>
    <header>
      <nav class="navbar navbar-expand-md navbar-dark fixed-top bg-dark">
        <a href="#" class="navbar-brand d-flex align-items-center">
          <img src="/static/image.png" width="20" height="20">
          <strong>Alurapic</strong>
        </a>
        <div class="collapse navbar-collapse" id="navbarCollapse">
          <meu-menu :rotas="routes"/>
        </div>
      </nav>
    </header>
    <section class="py-5 text-center container">
      <transition name="pagina">
        <router-view></router-view>
      </transition>
    </section>
  </div>
</template>

<script>

import { routes } from './routes';
import Menu from './components/shared/menu/Menu.vue';

export default {

  components: {
    'meu-menu' : Menu
  },
  
  data() {

    return {

      routes : routes.filter(route => route.menu)
    }
  }
}
</script>

<style>
  .corpo {
    font-family: Helvetica, sans-serif;
    width: 96%;
    margin: 0 auto;
  }

 .pagina-enter, .pagina-leave-active {

     opacity: 0;
 }

 .pagina-enter-active, .pagina-leave-active {

     transition: opacity .4s;
 }

</style>

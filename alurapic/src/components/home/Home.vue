<template>
  <main>
    <section class="py-5 text-center container">
      <div class="row py-lg-5">
        <div class="col-lg-6 col-md-8 mx-auto">
          <h1 class="fw-light">Alurapic</h1>
          <p v-show="mensagem" class="centralizado">{{ mensagem }}</p>
          <p class="lead text-muted">Buscar imagem.</p>
          <input class="form-control mr-sm-2" type="text" @input="filtro = $event.target.value" placeholder="filtre por parte do título">
        </div>
      </div>
    </section>

    <div class="album py-5 bg-light">
      <div class="container">
        <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 g-3">
          <div class="col-md-4" v-for="foto of fotosComFiltro">
            <div class="card mb-4 shadow-sm">
              <imagem-responsiva class="bd-placeholder-img card-img-top" v-meu-transform:scale.animate="1.1" :url="foto.url" :titulo="foto.titulo" width="100%" height="225" role="img"/>
              <div class="card-body">
                <h5 class="card-title">{{ foto.titulo }}</h5>
                <p class="card-text text-justify">{{ foto.descricao.substring(0, 150) + ' ...' }}</p>
                <div class="d-flex justify-content-between align-items-center">
                  <div class="btn-group">
                    <router-link :to="{ name: 'visualizar', params: { id: foto._id } }" class="btn btn-sm btn-info" type="button">Visualizar</router-link>
                    <router-link :to="{ name: 'altera', params: { id: foto._id } }" class="btn btn-sm btn-warning">Editar</router-link>
                    <button @click="remove(foto)" class="btn btn-sm btn-danger" type="button">Excluir</button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</main>
  
</template>

<script>
import Painel from '../shared/painel/Painel.vue';
import ImagemResponsiva from '../shared/imagem-responsiva/ImagemResponsiva.vue';
import FotoService from '../../domain/foto/FotoService'; 

export default {

  components: {
    // 'meu-painel' : Painel, 
    'imagem-responsiva': ImagemResponsiva,
  },

  data() {

    return {

      titulo: 'Alurapic', 
      fotos: [], 
      filtro: '',
      mensagem: ''
    }
  },

  computed: {

    fotosComFiltro() {

      if(this.filtro) {
        let exp = new RegExp(this.filtro.trim(), 'i');
        return this.fotos.filter(foto => exp.test(foto.titulo));
      } else {
        return this.fotos;
      }
    }
  },

  methods: {
    remove(foto) {

      this.service.apaga(foto._id)
        .then(() => {
            let indice = this.fotos.indexOf(foto);
            this.fotos.splice(indice, 1)
            this.mensagem = "Foto removida com sucesso";
        }, err => this.mensagem = err.message);
    }
  },

  created() {

    this.service = new FotoService(this.$resource);
    this.service.lista().then(fotos => this.fotos = fotos, err => this.mensagem = err.message);
  }
}
</script>

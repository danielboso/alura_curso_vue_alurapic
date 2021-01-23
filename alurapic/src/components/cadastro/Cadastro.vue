<!-- alurapic/src/components/cadastro/Cadastro.vue -->

<template>
  <section class="py-5 text-center container">
    <div class="row">
      <div class="col-md-12">
        <div class="card">
          <div v-if="!foto._id" class="card-header">Cadastrar</div>
          <div v-else class="card-header">Editar {{ foto.titulo }}</div>
          <div class="card-body">
            <form @submit.prevent="store()" novalidate>
              <div class="form-group">
                <label for="titulo">Título</label>
                <input name="titulo" type="text" class="form-control" :class="errors.has('titulo') ? 'is-invalid' : 'is-valid'" data-vv-as="título" v-validate data-vv-rules="required|min:3|max:30" autocomplete="off" v-model="foto.titulo">
                <div class="invalid-feedback" v-show="errors.has('titulo')">{{ errors.first('titulo') }}</div>
              </div>
              <div class="form-group">
                <label for="url">Url</label>
                <input class="form-control" v-validate data-vv-rules="required" name="url" autocomplete="off" v-model="foto.url" :class="errors.has('url') ? 'is-invalid' : 'is-valid'">
                <div class="invalid-feedback" v-show="errors.has('url')">{{ errors.first('url') }}</div>
                <imagem-responsiva class="mt-3" v-show="foto.url" :url="foto.url" :titulo="foto.titulo"/>
              </div>
              <div class="form-group">
                <label for="descricao">Descrição</label>
                <textarea id="descricao" class="form-control" autocomplete="off" v-model="foto.descricao"></textarea>
              </div>
            </form>
          </div>
          <div class="card-footer text-center">
            <router-link :to="{ name: 'home' }" type="button" class="btn btn-warning">Voltar</router-link>
            <button class="btn btn-success">Salvar</button>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>

import ImagemResponsiva from '../shared/imagem-responsiva/ImagemResponsiva.vue'
import Foto from '../../domain/foto/Foto';
import FotoService from '../../domain/foto/FotoService';

export default {

  components: {

    'imagem-responsiva': ImagemResponsiva, 
  }, 

  data() {
    return {
        foto: new Foto(),
        id: this.$route.params.id
    }
  },

  created() {
    this.service = new FotoService(this.$resource);

    if(this.id) {
      this.service.busca(this.id).then(foto => this.foto = foto)
    }
  },

  methods: {

    store() {

      this.$validator.validateAll().then(success => {

        if(success) {
          this.service.cadastra(this.foto)
            .then(() => {
              if(this.id) {
                this.$router.push({ name: 'home' })
              }
              this.foto = new Foto()
            }, err => console.log(err));
        }
      });
    }
  }
}

</script>
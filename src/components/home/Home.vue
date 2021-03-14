<template>
  <div>
    <h1 class="centralizado">{{ titulo }}</h1>

    <p v-show="mensagem" class="centralziado"> {{ mensagem }}</p>

    <input @input="filtro = $event.target.value"
      type="search" class="filtro"
      placeholder="filtre por parte do título"
    >

    <ul class="lista-fotos">
      <li class="lista-fotos-item" v-for="foto, i in fotosFiltradas" :key="i">

        <meu-painel :titulo="foto.titulo">
          <imagem-responsiva
            v-meu-transform:scale.animate="1.2"
            :url="foto.url"
            :titulo="foto.titulo"
          />
          <meu-botao
            tipo="button" rotulo="Remover"
            @botaoAtivado="remove(foto)"
            :confirmacao="true"
            estilo="perigo"
          />
        </meu-painel>

      </li>
    </ul>
  </div>
</template>

<script>
import Painel from '../shared/painel/Painel.vue'
import ImagemResponsiva from '../shared/imagem-responsiva/ImagemResponsiva.vue'
import Button from '../shared/buttons/Button.vue'

import transform from '../../directives/Transform';
import FotoService from '../domain/foto/FotoService';

export default {
  components: {
    'meu-painel' : Painel,
    'imagem-responsiva': ImagemResponsiva,
    'meu-botao': Button
  },
  directives: {
    'meu-transform': transform
  },
  data() {
    return {
      filtro: '',
      titulo: 'Alurapic',
      fotos: [],
      mensagem: '',
      resource: ''
    }
  },
  computed: {
    fotosFiltradas() {
      if (this.filtro) {
        let exp = new RegExp(this.filtro.trim(), 'i');
        return this.fotos.filter(foto => exp.test(foto.titulo));
      } else {
        return this.fotos;
      }
    }
  },
  created(){
    this.service = new FotoService(this.$resource)

    this.service
      .lista()
      .then(fotos => this.fotos = fotos, err => console.log(err));
  },
  methods: {
    remove(foto) {
      this.service.apaga(foto._id)
        .then(() => {
          let indice = this.fotos.indexOf(foto)
          this.fotos.splice(indice, 1)
          this.mensagem = 'Foto removida com sucesso'
          },
          err => {
            console.log(err)
            this.mensagem = 'Não foi possível remover a foto'
          }
        )
    }
  }
}
</script>

<style>

.centralizado {
  text-align: center;
}

.lista-fotos {
  list-style: none;
}

.lista-fotos .lista-fotos-item {
  display: inline-block;
}

.filtro {
  display: block;
  width: 100%
}
</style>

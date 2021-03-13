<template>
  <div>
    <h1 class="centralizado">{{ titulo }}</h1>

    <input @input="filtro = $event.target.value"
      type="search" class="filtro"
      placeholder="filtre por parte do título"
    >

    <ul class="lista-fotos">
      <li class="lista-fotos-item" v-for="foto, i in fotosFiltradas" :key="i">

        <meu-painel :titulo="foto.titulo">
          <imagem-responsiva :url="foto.url" :titulo="foto.titulo"/>
        </meu-painel>

      </li>
    </ul>
  </div>
</template>

<script>
import Painel from '../shared/painel/Painel.vue'
import ImagemResponsiva from '../shared/imagem-responsiva/ImagemResponsiva.vue'

export default {
  components: {
    'meu-painel' : Painel,
    'imagem-responsiva': ImagemResponsiva
  },
  data() {
    return {
      filtro: '',
      titulo: 'Alurapic',
      fotos: []
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
    let promisse = this.$http.get('http://localhost:3000/v1/fotos');
    promisse
      .then(res => res.json())
      .then(fotos => this.fotos = fotos, err => console.log(err));
  },
  methods: {}
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

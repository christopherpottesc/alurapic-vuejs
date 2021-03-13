<template>
  <button @click="disparaAcao()" class="button" :class="estiloBotao" :type="tipo">
    {{ rotulo }}
  </button>
</template>

<script>
export default {
  props: {
    tipo: {
      type: String,
      required: true
    },
    rotulo: {
      type: String,
      required: true
    },
    confirmacao: Boolean,
    estilo: String
  },
  computed: {
    estiloBotao() {
      if(this.estilo == "padrao" || !this.estilo) return 'button-default'

      if(this.estilo == "perigo") return 'button-alert';
    }

  },
  methods: {
    disparaAcao() {
      if(this.confirmacao) {
        if(confirm('Confirma a operação?')) {
          this.$emit('botaoAtivado')
        }
        return
      }
      this.$emit('botaoAtivado')
    }
  }
}
</script>

<style scoped>
.button {
  display: inline-block;
  padding: 10px;
  border-radius: 3px;
  margin: 10px;
  font-size: 1.2em;
}

.button-alert {
  background: firebrick;
  color: #fff;
}

.button-default {
  background: darkcyan;
  color: #fff;
}

</style>
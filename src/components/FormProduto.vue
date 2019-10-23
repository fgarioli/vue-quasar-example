<template>
  <form @submit.prevent="enviar">
    <div class="q-pa-md" style="max-width: 800px">
      <div class="row">
        <div class="col-12">
          <q-input outlined v-model="produto.nome" label="Nome" />
        </div>
        <div class="col-12">
          <q-input outlined v-model="produto.descricao" label="Descrição" />
        </div>
        <div class="col-3">
          <q-input outlined v-model="produto.preco" label="Preço" />
        </div>
        <div class="col-3">
          <q-input outlined v-model="produto.quantidade" label="Quantidade" />
        </div>
      </div>
      <div class="row justify-end">
        <q-btn
          :loading="loading"
          color="primary"
          class="q-mt-md"
          type="submit"
          style="width: 150px"
        >
          Enviar
          <template v-slot:loading>
            <q-spinner-hourglass class="on-left" />Loading...
          </template>
        </q-btn>
      </div>
    </div>
    <q-dialog v-model="persistent" persistent transition-show="scale" transition-hide="scale">
      <q-card class="bg-teal text-white" style="width: 300px">
        <q-card-section>
          <div class="text-h6">Aviso</div>
        </q-card-section>

        <q-card-section>Produto Cadastrado com sucesso!</q-card-section>

        <q-card-actions align="right" class="bg-white text-teal">
          <q-btn flat label="OK" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </form>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      produto: {},
      loading: false,
      persistent: false
    };
  },
  methods: {
    async enviar() {
      try {
        this.loading = true;
        await new Promise(resolve => setTimeout(resolve, 2000));
        await axios.post("localhost", this.produto);
        this.persistent = true;
      } catch (error) {
        this.loading = false;
        throw error;
      } finally {
        this.loading = false;
      }
    }
  }
};
</script>

<style lang="sass" scoped>
.row > div
  padding: 5px 10px
.row + .row
  margin-top: 1rem
</style>
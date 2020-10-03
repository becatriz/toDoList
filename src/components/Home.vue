<template>
  <div>
    <v-row>
    <v-spacer></v-spacer>
      <v-col class="auto" cols="12" sm="6" md="6">
        <v-text-field v-model="tituloBusca" label="Busca por Título" append-icon="mdi-magnify" rounded outlined></v-text-field>
      </v-col>
    </v-row>
    <v-row justify="center" class="my-5" v-if="loading">
      <v-col cols="auto">
        <v-progress-circular indeterminate color="green"></v-progress-circular>
      </v-col>
    </v-row>
    <v-row v-else>
      <v-col
        cols="12"
        md="6"
        lg="3"
        xl="2"
        v-for="item in tarefas"
        :key="item.titulo"
      >
        <v-card class="elevation-1">
          <v-card-title
            >{{ item.titulo }}
            <v-spacer></v-spacer>
            <v-btn @click="editarTarefa(item)" icon small>
              <v-icon color="green">mdi-pencil</v-icon>
            </v-btn>
          </v-card-title>
          <v-card-text>{{ item.descricao }}</v-card-text>
        </v-card>
      </v-col>
    </v-row>
    <v-speed-dial absolute bottom right>
      <template v-slot:activator>
        <v-btn fab color="green" dark @click="novaTarefa">
          <v-icon>mdi-plus</v-icon>
        </v-btn>
      </template>
    </v-speed-dial>
    <v-dialog width="500" v-model="dialog">
      <v-card>
        <v-card-title>Nota Tarefa</v-card-title>
        <v-card-text>
          <v-form>
            <v-row dense>
              <v-col sm="6" md="12" cols="12">
                <v-text-field
                  v-model="tarefa.titulo"
                  type="text"
                  label="Título"
                  outlined
                  rounded
                ></v-text-field>
              </v-col>
              <v-col sm="6" md="12" cols="12">
                <v-text-field
                  v-model="tarefa.descricao"
                  type="text"
                  label="Descrição"
                  outlined
                  rounded
                ></v-text-field>
              </v-col>
              <v-col sm="6" md="12" cols="12">
                <v-btn
                  :loading="salvando"
                  color="green"
                  class="elevation-0"
                  dark
                  block
                  rounded
                  @click="salvar"
                  >Salvar</v-btn
                >
              </v-col>
              <v-col sm="6" md="12" cols="12">
                <v-btn
                  :disabled="salvando"
                  class="elevation-0"
                  text
                  dark
                  block
                  rounded
                  color="error lighten-3"
                  @click="removerTarefa"
                  >Excluir Tarefa</v-btn
                >
              </v-col>
            </v-row>
          </v-form>
        </v-card-text>
      </v-card>
    </v-dialog>
  </div>
</template>
<script>
import axios from "axios";

export default {
  data() {
    return {
      dialog: false,
      loading: true,
      salvando: false,
      tarefa: {},
      tarefas: [],
      tituloBusca: ''
    };
  },

  watch:{
    tituloBusca: function (tituloAtual, tituloAnterior) {
        this.buscarTarefas({titulo: tituloAtual })
    }
  },

  methods: {
    novaTarefa() {
      this.dialog = true;
    },
    editarTarefa(item) {
      this.tarefa = JSON.parse(JSON.stringify(item));
      this.dialog = true;
    },
    async removerTarefa() {
       /*----- Forma de enviar requisições completas -----------
        await axios.request({
         url:'http://localhost:3000/tarefas',
         method: 'delete',
         data: this.tarefa
       });
      --------------------------------------------------------------*/
      await axios.delete(`http://localhost:3000/tarefas/${this.tarefa._id}`);
      this.dialog = false;
      this.buscarTarefas();
    },
    async salvar() {
      this.salvando = true;
      if (this.tarefa._id)
        await axios.put("http://localhost:3000/tarefas", this.tarefa);
      else await axios.post("http://localhost:3000/tarefas", this.tarefa);
      this.buscarTarefas();
      this.tarefa = {};
      this.salvando = false;
      this.dialog = false;
    },
    async buscarTarefas(filtro) {
      this.loading = true;
      let resposta;
      if(!filtro) resposta = await axios.get("http://localhost:3000/tarefas");
      else resposta = await axios.get(`http://localhost:3000/tarefas?titulo=${filtro.titulo}`);
      this.tarefas = resposta.data;
      this.loading = false;
    },
  },
  mounted() {
    this.buscarTarefas();
  },
};
</script>



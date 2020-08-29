<template>
  <div>
       <v-card 
          class="mx-auto"
          max-width="800"  v-for="item in tarefas" :key="item.titulo">
          <v-card-title>{{ item.titulo }}</v-card-title>
          <v-card-text>{{ item.descricao }}</v-card-text>  
       </v-card>
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
                <v-text-field v-model="tarefa.titulo" type="text" label="Título" outlined rounded></v-text-field>
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
                <v-btn color="green" class="elevation-0" dark block rounded @click="salvar">Salvar</v-btn>
              </v-col>
            </v-row>
          </v-form>
        </v-card-text>
      </v-card>
    </v-dialog>
  </div>
</template>
<script>
export default {
  data() {
    return {
      dialog: false,
      tarefa: {},
      tarefas: [],
    };
  },
  methods: {
    novaTarefa() {
      this.dialog = true;
    },
    salvar() {
      this.tarefas.push(this.tarefa);
      this.tarefa = {};
      this.dialog = false;
    },
  },
};
</script>


<template>
  <v-data-table
    :headers="headers"
    :items="albuns"
    :options.sync="opcoes"
    :server-items-length="totalAlbuns"
    :loading="carregando"
    loading-text="Carregando... Por favor, aguarde."
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar flat>
        <v-dialog v-model="dialog" max-width="800px">
          <v-card>
            <v-card-title>
              <span class="headline">Detalhes do Álbum</span>
            </v-card-title>
            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12" sm="6" md="6">
                    <b>Álbum: </b
                    ><span v-if="albumVisualizado">{{
                      albumVisualizado.nome
                    }}</span>
                    <br />
                    <b>Artista: </b
                    ><span v-if="albumVisualizado">{{
                      albumVisualizado.artista.nome
                    }}</span>
                  </v-col>
                  <v-col cols="12" sm="6" md="6">
                    <v-carousel v-model="capaAtual" v-if="capasVisualizado">
                      <v-carousel-item
                        v-for="capa in capasVisualizado"
                        :key="capa"
                      >
                        <v-img :src="capa"
                        >
                          <template v-slot:placeholder>
                            <v-row
                              class="fill-height ma-0"
                              align="center"
                              justify="center"
                            >
                              <v-progress-circular
                                indeterminate
                                color="grey lighten-5"
                              ></v-progress-circular>
                            </v-row>
                          </template>
                        </v-img>
                      </v-carousel-item>
                    </v-carousel>
                    <div v-else>
                      <v-icon large> mdi-image-off </v-icon>
                      <span>Nenhuma capa disponível</span>
                    </div>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="fechar"> Fechar </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:item.actions="{ item }">
      <v-icon class="mr-2" @click="detalhes(item)"> mdi-file-find </v-icon>
    </template>
  </v-data-table>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      headers: [
        { text: "Álbum", value: "nome", sortable: false },
        { text: "Artista", value: "artista.nome", sortable: false },
        { text: "Ações", value: "actions", sortable: false },
      ],
      albuns: [],
      totalAlbuns: 0,
      carregando: true,
      opcoes: {},
      dialog: false,
      albumVisualizado: null,
      capasVisualizado: null,
      capaAtual: 0,
      colors: ["primary", "secondary", "yellow darken-2", "red", "orange"],
    };
  },
  watch: {
    opcoes: {
      handler() {
        this.getAlbuns();
      },
      deep: true,
    },
  },
  methods: {
    getAlbuns() {
      this.carregando = true;
      this.$http
        .get(
          `/album/?page=` +
            (this.opcoes.page - 1) +
            `&pageSize=` +
            (this.opcoes.itemsPerPage == -1
              ? this.totalAlbuns
              : this.opcoes.itemsPerPage)
        )
        .then((response) => {
          this.albuns = response.data.albuns;
          this.totalAlbuns = response.data.totalAlbuns;
          this.carregando = false;
        })
        .catch((e) => {
          console.log(e);
          this.$router.push({ name: "login" });
        });
    },
    getCapas(albumUID)
    {
        this.$http
        .get(
          `/capa/` + albumUID)
        .then((response) => {
          this.capasVisualizado = response.data;          
        })
        .catch((e) => {
          console.log(e);          
        });
    },
    fechar() {
      this.albumVisualizado = null;
      this.capasVisualizado = null;
      this.capaAtual = 0;
      this.dialog = false;
    },
    detalhes(item) {
      this.albumVisualizado = item;
      this.dialog = true;
      this.getCapas(item.albumUID)
    }        
  },
};
</script>


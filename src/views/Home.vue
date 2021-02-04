<template>
  <v-data-table
    :headers="headers"
    :items="albuns"
    :options.sync="opcoes"
    :server-items-length="totalAlbuns"
    :loading="carregando"
    loading-text="Carregando... Por favor, aguarde."
    class="elevation-1"
  ></v-data-table>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      headers: [
        { text: "Álbum", value: "nome", sortable: false },
        { text: "Artista", value: "artista.nome", sortable: false },
        { text: "Detalhes", value: "albumUID", sortable: false },
      ],
      albuns: [],
      totalAlbuns: 0,
      carregando: true,
      opcoes: {},
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
      this.$http.get(`/album/?page=`+ (this.opcoes.page - 1) +`&pageSize=` + (this.opcoes.itemsPerPage == -1 ? this.totalAlbuns : this.opcoes.itemsPerPage))
        .then((response) => {
          console.log(response.data);
          this.albuns = response.data.albuns;
          this.totalAlbuns = response.data.totalAlbuns;
          this.carregando = false;
        })
        .catch((e) => {
          console.log(e);
        });      
    }
  }
};
</script>


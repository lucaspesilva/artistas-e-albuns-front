<template>
  <v-data-table
    :headers="headers"
    :items="albuns"
    :items-per-page="10"
    class="elevation-1"
  ></v-data-table>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      headers: [          
          { text: 'Álbum', value: 'nome', sortable: false },
          { text: 'Artista', value: 'artista.nome', sortable: false },
          { text: 'Detalhes', value: 'albumUID', sortable: false },
        ],
      albuns: []
    };
  },
  created() {
    this.$http
      .get(`http://localhost:8080/album/?page=0&pageSize=10`)
      .then((response) => {
        console.log(response.data);
        this.albuns = response.data;
      })
      .catch((e) => {        
        console.log(e);
      });
  },
};
</script>


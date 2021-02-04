<template>
  <v-app>
    <Navbar />
    <v-main class="ma-5">
      <router-view @onLogar="UsuarioLogado">        
      </router-view>
    </v-main>
  </v-app>
</template>

<script>
import Navbar from "@/components/Navbar";
export default {
  name: "App",
  components: { Navbar },  
  methods: {
    UsuarioLogado(token) {
      this.$http.defaults.headers.common['Authorization'] =  token;
      this.$router.push({name: "home"});
    }
  }
};
</script>

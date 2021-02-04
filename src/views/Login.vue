<template>
  <v-dialog v-model="dialog" persistent max-width="600px" min-width="360px">
    <div>
      <v-tabs background-color="deep-purple accent-4" icons-and-text dark grow>
        <v-tabs-slider color="purple darken-4"></v-tabs-slider>
        <v-tab>
          <v-icon large>mdi-account</v-icon>
          <div class="caption py-1">Login</div>
        </v-tab>
        <v-tab-item>
          <v-card class="px-4">
            <v-card-text>
              <v-form ref="loginForm" v-model="valid" lazy-validation>
                <v-row>
                  <v-col cols="12">
                    <v-text-field
                      v-model="user"
                      label="Usuário"
                      :rules="[rules.required]"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12">
                    <v-text-field
                      v-model="senha"
                      :rules="[rules.required]"
                      type="password"
                      name="input-10-1"
                      label="Senha"
                      counter
                    ></v-text-field>
                  </v-col>
                  <v-col class="d-flex" cols="12" sm="6" xsm="12"> 
                    {{ erro }}
                  </v-col>
                  <v-spacer></v-spacer>
                  <v-col class="d-flex" cols="12" sm="3" xsm="12" align-end>
                    <v-btn
                      x-large
                      block
                      :disabled="!valid"
                      color="success"
                      @click="logar"
                    >
                      Login
                    </v-btn>
                  </v-col>
                </v-row>
              </v-form>
            </v-card-text>
          </v-card>
        </v-tab-item>
      </v-tabs>
    </div>
  </v-dialog>
</template>

<script>
export default {
  methods: {
    logar() {
      if (this.$refs.loginForm.validate()) {
        this.valid = false;
        this.$http
          .post(`/login`, {
            username: this.user,
            password: this.senha,
          })
          .then((response) => 
          {
            this.$emit("onLogar", response.headers["authorization"]);
          })
          .catch((e) => 
          {
            this.erro = e.message;
            this.valid = true;
          });
      }
    },
  },
  data: () => ({
    dialog: true,
    valid: false,
    user: "",
    senha: "",
    erro: "",
    rules: {
      required: (value) => !!value || "Obrigatório.",
    },
  }),
};
</script>
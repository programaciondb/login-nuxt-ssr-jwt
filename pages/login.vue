<template>
  <v-form @submit.prevent="userLogin">
    <v-container>
      <v-row>
        <v-col cols="12" md="4">
          <v-text-field
            name="name"
            label="Username"
            v-model="login.username"
            required
            :rules="usernameRules"
          ></v-text-field>
        </v-col>
        <v-col cols="12" md="4">
          <v-text-field
            name="password"
            label="Password"
            v-model="login.password"
            required
            :rules="passRules"
            type="password"
          ></v-text-field>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="12" md="4">
          <button type="submit">Submit</button>
        </v-col>
      </v-row>
    </v-container>
  </v-form>
</template>

<script>
export default {
  data() {
    return {
      login: {
        username: "",
        password: "",
      },
      usernameRules: [
          v => !!v || 'Ingrese username',
          v => /.+@.+/.test(v) || 'Debe ser un correo correcto',
      ],
      passRules: [
          v => !!v || 'Ingrese username',
          v => v.length >= 5 || 'Mínimo 5 caracteres',
      ]
    };
  },
  methods: {
    async userLogin() {
      try {
        await this.$auth.loginWith("local", {
          data: this.login,
        });
      } catch (err) {
        console.log(err);
      }
    },
  },
};
</script>

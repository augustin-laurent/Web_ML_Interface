<template>
    <v-app-bar>
        <v-app-bar-nav-icon>
            <img src="https://static.thenounproject.com/png/2300300-200.png" alt="Icone" style="width: 100%;"/>
        </v-app-bar-nav-icon>
        <v-app-bar-title class="white--text">Machine learning Tools</v-app-bar-title>
        <v-spacer />
        <v-app-bar-items>
            <v-btn flat @click="setComponent('SandBoxLayout')">SandBox</v-btn>
            <v-btn flat @click="setComponent('DatasetsLayout')">Datasets</v-btn>
            <v-btn flat @click="setComponent('ModelsLayout')">Models</v-btn>
            <v-btn flat @click="loginForm = true" color="blue" v-if="loggedIn === false"><v-icon>mdi-account</v-icon>Login</v-btn>
            <v-btn v-if="loggedIn === true" @click="logout">{{ username }}</v-btn>
        </v-app-bar-items>
    </v-app-bar>

    <v-dialog v-if="register === false" v-model="loginForm" max-width="500px">
      <v-card>
        <v-card-title>
          <span class="text-h5">Connexion</span>
        </v-card-title>
        <form ref="loginForm" method="POST" onsubmit="return false;" id="loginForm">
          <v-card-text>
            <v-text-field type="input" v-model="username" label="Nom de compte" name="username" ref="username" required/>
          </v-card-text>
          <v-card-text>
            <v-text-field type="password" v-model="password" label="Mot de passe" name="password" ref="password" required/>
          </v-card-text>
          <v-container>
            <v-btn block color="blue darken-1" type="submit" form="loginForm" @click="login">Connexion</v-btn>
          </v-container>
        </form>
        <v-container>
          <v-btn flat color="blue" @click="register = true">Créer compte</v-btn>
        </v-container>
      </v-card>
    </v-dialog>

    <v-dialog v-if="register === true" v-model="loginForm" max-width="500px">
      <v-card>
        <v-card-title>
          <span class="text-h5">Créer un compte</span>
        </v-card-title>
        <form ref="loginForm" method="POST" onsubmit="return false;" id="loginForm">
          <v-card-text>
            <v-text-field type="input" v-model="username" label="Nom de compte" name="username" ref="username" required/>
          </v-card-text>
          <v-card-text>
            <v-text-field type="password" v-model="password" label="Mot de passe" name="password" ref="password" required/>
          </v-card-text>
          <v-card-text>
            <v-text-field type="password" v-model="passwordConfirm" label="Mot de passe" name="password" ref="password" required/>
          </v-card-text>
          <v-container>
            <v-btn block color="blue darken-1" type="submit" form="loginForm" @click="register">Enregistrer</v-btn>
          </v-container>
        </form>
        <v-container>
          <v-btn flat color="blue" @click="register = false">Se connecter</v-btn>
        </v-container>
      </v-card>
    </v-dialog>
</template>

<script>
export default {
  data() {
    return {
      loginForm: false,
      username: null,
      password: null,
      passwordConfirm: null,
      loggedIn: false,
      register: false,
    }
  },
  created() {
    fetch('/api/myname')
    .then((res) => res.json()).then((data) => { if(data === null) { this.loggedIn = false } else {this.loggedIn = true; this.username = data}})
    this.$emit('isLoggedin', this.loggedIn)
  },
  methods: {
    login() {
      fetch('/api/login', { method:"POST", body: JSON.stringify({ username: this.username, password: this.password })})
      .then(
        this.loggedIn = true,
        this.loginForm = false
      )
    },
    register() {
      if(this.password === this.passwordConfirm) {
        fetch('/api/register', { method:"POST", body: JSON.stringify({username: this.username, password: this.password})})
      }
      else {
        console.log("Les mots de passe ne correspondent pas")
      }
    },
    logout() {
      fetch('/api/logout')
      .then(
        this.loggedIn = false,
        this.username = null,
        this.password = null,
      )
    },
    setComponent(component) {
      this.$emit('change-component', component);
    },
  },
};
</script>
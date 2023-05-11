<template>
  <div class="login-container">
    <form class="login-form">
      <div class="name-app">
        <h1 class="login-name-app">PapApp Cost</h1>
      </div>
      <h2 class="login-title">Iniciar Sesión</h2>
      <div class="form-group">
        <label for="username" class="form-label">Usuario</label>
        <input id="username" type="text" class="form-input" placeholder="Ingresa tu usuario" v-model="username">
      </div>
      <div class="form-group">
        <label for="password" class="form-label">Contraseña</label>
        <input id="password" type="password" class="form-input" placeholder="Ingresa tu contraseña" v-model="password">
      </div>
      <div class="form-group">
        <button class="btn btn-primary" @click.prevent="login()" :disabled="loading">Ingresar</button>
        <button class="btn btn-secondary" @click.prevent="register()" :disabled="loading">Regístrate</button>
      </div>
      <div>
        <span v-if="loading" class="loading-message">
          <img src="./assets/time_clock.png" alt="Loading..." class="loading-icon">
        </span>
      </div>
      <div class="form-group">
        <a href="#" class="forgot-password-link">¿Olvidaste tu contraseña?</a>
      </div>
    </form>
  </div>
</template>

<script>

import axios from 'axios'

export default {
  data() {
    return {
      username: '',
      password: '',
      loading: false,
      userData: [],
    }
  },
  methods: {
    async login() {
      // Getting the values of username and password to sent to the API 
      try {
        this.loading = true;
        const response = await axios.post('https://web-services-papappcost-umb.onrender.com/login/auth', {
          email_user: this.username,
          password_user: this.password
        });
        this.userData = response.data;

        if (this.userData['id_user'] > 0) {
          console.log(this.userData['id_user'])
        } else {
          this.$swal({
            icon: 'error',
            title: 'Oops...',
            text: 'Email o contraseña incorrecta.'
          });
        }

      } catch (error) {
        console.error(error);
      } finally {
        this.loading = false;
      }
    },
    register() {
      // testing about consult of user data from API
    },
    testAPI() {
      // testing about consult of user data from API
      axios.get('https://web-services-papappcost-umb.onrender.com/login/signup/1')
        .then(response => {
          // Saving the user data from API in a list
          this.userData = response.data
          console.log(response.data);
        })
        .catch(error => {
          console.log(error);
        })
    }
  }
}
</script>

<style>
.login-container {
  display: flex;
  justify-content: center;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  align-items: center;
  height: 100vh;
  background-image: url('./assets/campo.jpg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}

.login-form {
  background-color: #f9f2f4;
  padding: 40px;
  border-radius: 5px;
  box-shadow: 0px 3px 15px rgba(0, 0, 0, 0.2);
  text-align: center;
  max-width: 400px;
  width: 100%;
}

.login-title {
  font-size: 24px;
  font-weight: bold;
  color: #268604;
  margin-bottom: 20px;
}

.name-app {
  font-size: 28px;
  font-weight: bold;
  color: #268604;
  margin-bottom: 20px;
}

.form-group {
  margin-bottom: 20px;
}

.form-label {
  font-size: 16px;
  font-weight: bold;
  display: block;
  margin-bottom: 5px;
  color: #268604;
}

.form-input {
  border: none;
  border-radius: 3px;
  padding: 10px;
  font-size: 16px;
  width: 100%;
  background-color: #f7fff4;
  margin-bottom: 10px;
}

.btn {
  border: none;
  border-radius: 3px;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  transition: all 0.2s ease;
}

.btn-primary {
  background-color: #268604;
  color: #f7fff4;
  margin-right: 10px;
}

.btn-secondary {
  background-color: #f7fff4;
  color: #268604;
  border: 1px solid #268604;
}

.btn-primary:hover,
.btn-secondary:hover {
  transform: translateY(-2px);
}

.forgot-password-link {
  font-size: 14px;
  color: #268604;
}

.loading-icon {
  width: 40px;
  height: 40px;
  margin-right: 5px;
  vertical-align: middle;
  background-color: #f7fff4;
  animation: spinner 0.8s infinite linear;
}

@keyframes spinner {
  to { transform: rotate(360deg); }
}
</style>

<template>
  <div class="login">
    <h2>{{'auth.welcome' | translate}}</h2>
    <form @submit.prevent="loginUser">
      <div class="form-group">
        <div class="input-group">
          <input v-model="login.email" type="text" id="email" required="required"/>
          <label class="control-label" for="email">{{'auth.email' | translate}}</label><i class="bar"></i>
        </div>
      </div>
      <div class="form-group">
        <div class="input-group">
          <input v-model="login.password" type="password" id="password" required="required"/>
          <label class="control-label" for="password">{{'auth.password' | translate}}</label><i class="bar"></i>
        </div>
      </div>
      <div class="d-flex flex-column flex-lg-row align-items-center justify-content-between down-container">
        <button class="btn btn-primary" type="submit">
          {{'auth.login' | translate}}
        </button>
        <router-link class='link' :to="{name: 'signup'}">{{'auth.createAccount' | translate}}</router-link>
      </div>
    </form>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'login',
  data () {
    return {
      login: {
        email: '',
        password: '',
        remember_me: true,
      }
    }
  },
  created() {
    // var headers = {
    //     'Content-Type': 'application/json',
    //     'Authorization': 'XMLHttpRequest' 
    // }
    
  },
  methods: {
    loginUser() {
      var self = this;
      axios.post('http://192.168.10.10/api/auth/login', self.login)
      .then(response => {
        let token = response.data.access_token;
        if (token) {
          localStorage.setItem('token', token)
        }
        console.log(response.data);

      })
      .catch(error => {
        console.log(error);
      });
    }
  }
}
</script>

<style lang="scss">
  .login {
    @include media-breakpoint-down(md) {
      width: 100%;
      padding-right: 2rem;
      padding-left: 2rem;
      .down-container {
        .link {
          margin-top: 2rem;
        }
      }
    }

    h2 {
      text-align: center;
    }
    width: 21.375rem;

    .down-container {
      margin-top: 3.125rem;
    }
  }
</style>

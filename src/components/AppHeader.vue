<template>
	<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">Navbar</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
        <li class="nav-item">
          <router-link class="nav-link active" aria-current="page" to="/">Home</router-link>
        </li>
        <li class="nav-item">
          <router-link v-if="store.token == ''" class="nav-link" to="/login">Login</router-link>
          <a href="javascript:void(0);" v-else class="nav-link" @click="logout">Logout</a>
        </li>
      </ul>
    </div>
  </div>
</nav>
</template>

<script>
import { store } from './store.js'
import router from './../router/index.js'

export default {
  data() {
    return {
      store
    }
  },
  methods: {
    logout() {
      const payload = {
        token: store.token,
      }
      const requestOptions = {
        method: "POST",
        body: JSON.stringify(payload),
      }

			console.log(requestOptions.body)
      fetch("http://localhost:8081/users/logout", requestOptions)
      .then((response) => response.json())
      .then((response) => {
        if (response.error) {
          console.log(response.message);
        } else {
          store.token = "";
          router.push("/login");
        }
      })
    }
  }
}
</script>

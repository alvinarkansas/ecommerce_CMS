<template>
  <div class="signin-page">
    <Loading v-if="isLoading" />
    <form @submit.prevent="signIn">
      <div class="d-flex flex-column align-items-center">
        <img src="../../public/Shoesify-Logo.svg" style="width: 50px" class="mb-3" alt="Logo">
        <h2>Sign In As Admin</h2>
      </div>
    <div class="form-group mt-3">
      <label for="exampleInputEmail1">Email address</label>
      <input v-model="email" type="email" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp" placeholder="admin@gmail.com">
    </div>
    <div class="form-group">
      <label for="exampleInputPassword1">Password</label>
      <input v-model="password" type="password" class="form-control" id="exampleInputPassword1" placeholder="123123">
    </div>
    <button type="submit" class="btn btn-primary">Sign In</button>
  </form>
  </div>
</template>

<script>
import Loading from '../components/Loading'
export default {
  name: 'SigninPage',
  components: {
    Loading
  },
  data: function () {
    return {
      email: '',
      password: ''
    }
  },
  methods: {
    signIn: function () {
      this.$store.commit('SET_ISLOADING', true)
      this.$store.dispatch('signIn', { email: this.email, password: this.password })
        .then(({ data }) => {
          this.$vToastify.success('Successfully Signed In', 'Hello, Admin!')
          localStorage.setItem('token', data.token)
          this.$router.push('/dashboard')
        })
        .catch(err => {
          this.$vToastify.error(err.response.data, 'Login Error')
        })
        .finally(_ => {
          this.$store.commit('SET_ISLOADING', false)
        })
    }
  },
  computed: {
    isLoading: function () {
      return this.$store.state.isLoading
    }
  }
}
</script>

<style>
.signin-page {
  background: linear-gradient(180deg, rgba(100,100,100,1) 0%, rgba(43,43,43,1) 100%);
  color: white;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>

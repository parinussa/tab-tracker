<template>
  <v-layout column>
    <v-flex xs6 offset-xs3>
      <panel title="Login">
        <v-text-field
          name="email"
          label="Email address"
          v-model="email"
        ></v-text-field>
        <br>

        <v-text-field
          name="password"
          label="Password"
          type="password"
          v-model="password"
        ></v-text-field>
        <br>

        <div class="danger-alert" v-html="error" />

        <br>
        <v-btn
          dark
          class="cyan"
          @click="login">
          Login
        </v-btn>
      </panel>
    </v-flex>
  </v-layout>
</template>

<script>
import AuthenticationService from '@/services/AuthenticationService'

export default {
  data () {
    return {
      email: '',
      password: '',
      error: null
    }
  },
  methods: {
    async login () {
      try {
        const response = await AuthenticationService.login({
          email: this.email,
          password: this.password
        })

        this.$store.dispatch('setToken', response.data.token)
        this.$store.dispatch('setUser', response.data.user)
        this.$router.push({
          name: 'songs'
        })
      } catch (e) {
        this.error = e.response.data.error
      }
    }
  },
  components: {
  }
}
</script>

<style scoped>
</style>

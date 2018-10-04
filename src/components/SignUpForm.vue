<template>
  <b-form @submit="onSubmit">
    <b-form-group :state="state" :invalid-feedback="invalidFeedback">
      <b-form-input v-model="username" type="text" placeholder="Username" required></b-form-input>
    </b-form-group>

    <b-form-group>
      <b-form-input v-model="password" type="password" placeholder="Password" required></b-form-input>
    </b-form-group>

    <div class="text-center">
      <b-button type="submit" variant="primary" block>Sign up</b-button>
      <b-button variant="link" @click="$emit('swap-form')">Have an account already? Sign in</b-button>

    </div>
  </b-form>
</template>

<script>
import skygear from 'skygear'

export default {
  data () {
    return {
      username: '',
      password: '',
      state: null,
      invalidFeedback: ''
    }
  },
  methods: {
    onSubmit (evt) {
      evt.preventDefault()

      // Signup the user here
      skygear.auth.signupWithUsername(this.username, this.password)
        .then(user => {
          console.log(user)
          this.$emit('sign-in')
        })
        .finally(() => {
          this.$emit('async-end')
        })
    }
  }
}
</script>

<template>
  <b-container>
    <b-row align-h="center" align-v="center">
      <b-col cols="2">
        <b-img class="p-3" src="../assets/logo.png" fluid alt="hi"/>
      </b-col>
      <b-col cols="6">
        <h1>Skygear x Vue</h1>
        <p><small>A simple demo connecting to Skygear</small></p>
      </b-col>
    </b-row>

    <b-row align-h="center">
      <b-col cols="6">
        <sign-in-form v-if="wantSignIn" @swap-form="swapForm" @sign-in="$emit('sign-in')" @async-start="$emit('async-start')" @async-end="$emit('async-end')"/>
        <sign-up-form v-else @swap-form="swapForm" @sign-in="$emit('sign-in')" @async-start="$emit('async-start')" @async-end="$emit('async-end')"/>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import SignInForm from './SignInForm'
import SignUpForm from './SignUpForm'

export default {
  data () {
    return {
      wantSignIn: true
    }
  },
  methods: {
    swapForm () {
      this.wantSignIn = !this.wantSignIn
    }
  },
  components: {
    SignInForm,
    SignUpForm
  }
}
</script>

<template>
  <div>
    <b-navbar toggleable="md" type="dark" variant="info">

      <b-navbar-toggle target="nav_collapse"></b-navbar-toggle>

      <b-navbar-brand href="#">Vue login Project</b-navbar-brand>

      <b-collapse is-nav id="nav_collapse">

        <!-- Right aligned nav items -->
        <b-navbar-nav class="ml-auto">
          <b-nav-item-dropdown right v-if="signedIn">
            <!-- Using button-content slot -->
            <template slot="button-content">
              User
            </template>
            <b-dropdown-item @click="signOut">Signout</b-dropdown-item>
          </b-nav-item-dropdown>
        </b-navbar-nav>

      </b-collapse>
    </b-navbar>
    <b-progress v-show="asyncState" :value="100" variant="secondary" animated :striped="false"></b-progress>
    <b-container v-if="!signedIn">
      <b-row align-v="center" style="min-height: 100vh;">
          <b-col>
            <auth-page @sign-in="signIn" @async-start="onAsyncStart" @async-end="onAsyncEnd"/>
          </b-col>
      </b-row>
    </b-container>
    <main-page v-else @sign-out="signOut" @async-start="onAsyncStart" @async-end="onAsyncEnd"/>
  </div>
</template>

<script>
import skygear from 'skygear'
import AuthPage from './components/AuthPage'
import MainPage from './components/MainPage'

const SKYGEAR_ENDPOINT = 'https://myvuedemo.skygeario.com/'
const SKYGEAR_APIKEY = '2b6c7de0200a4bfbafc6f225025f3e21'

export default {
  data () {
    console.log(skygear.auth.currentUser)
    return {
      asyncState: false,
      signedIn: false
    }
  },
  mounted: function () {
    skygear.config({
      'endPoint': SKYGEAR_ENDPOINT,
      'apiKey': SKYGEAR_APIKEY
    }).then(() => {
      console.log('skygear container is now ready for making API calls.')
      console.log(skygear.auth.currentUser)
      if (skygear.auth.currentUser) {
        this.signedIn = true
      }
    }, (error) => {
      console.error(error)
    })

    skygear.auth.onUserChanged(user => {
      if (user) {
        console.log('user logged in or signed up')
        this.signedIn = true
      } else {
        console.log('user logged out or the access token expired')
        this.signedIn = false
      }
    })
  },
  methods: {
    signIn () {
      this.signedIn = true
    },
    onAsyncStart () {
      this.asyncState = true
    },
    onAsyncEnd () {
      this.asyncState = false
    },
    signOut () {
      this.$emit('async-start')
      skygear.auth.logout()
        .then(() => {
          this.$emit('sign-out')
          this.signedIn = false
        })
        .catch(({ error }) => {
          console.error(error)
          this.setState({
            event: {
              type: 'error',
              message: error.message
            }
          })
        })
        .finally(() => {
          this.$emit('async-end')
        })
    }
  },
  components: {
    AuthPage,
    MainPage
  }
}
</script>

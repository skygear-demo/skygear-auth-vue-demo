<template>
  <b-container class="mt-3">

    <center><h1>Welcome {{user.username}}!</h1></center>

  </b-container>
</template>

<script>
import skygear from 'skygear'

export default {
  components: {
  },
  data () {
    return {
      user: skygear.auth.currentUser
    }
  },
  mounted: function () {
  },
  methods: {
  }
}
</script>

<template>
   <div>
    <b-navbar style="height:100px">
        <template slot="brand">
            <b-navbar-item tag="router-link" :to="{ path: '/' }">
                <img
                    src="@/assets/img/buefy.png"
                >
            </b-navbar-item>
        </template>
        <template slot="start">
            <nuxt-link
            to="/index"
            exact-active-class="is-active"
            tag="b-navbar-item"
            >
              HOME
            </nuxt-link>
            <nuxt-link
            :to="`/map`"
            exact-active-class="is-active"
            tag="b-navbar-item"
            >
              MAP
            </nuxt-link>
            <nuxt-link
            :to="`/post`"
            exact-active-class="is-active"
            tag="b-navbar-item"
            >
              POST
            </nuxt-link>
            <nuxt-link
            :to="`/video`"
            exact-active-class="is-active"
            tag="b-navbar-item"
            >
              VIDEO
            </nuxt-link>
        </template>
        <template slot="end">
            <b-navbar-item @click="logout()">
                ログアウト
            </b-navbar-item>
        </template>
    </b-navbar>
    <nuxt />
  </div>
</template>

<script>
export default {
  data () {
    return {
      items: [
        {
          title: 'Home',
          icon: 'home',
          to: { name: 'index' }
        },
        {
          title: 'Inspire',
          icon: 'lightbulb',
          to: { name: 'inspire' }
        }
      ]
    }
  }
}
</script>

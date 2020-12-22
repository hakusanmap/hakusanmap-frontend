<template>
    <div>
        <b-navbar style="height:100px">
            <template slot="brand">
                <b-navbar-item tag="router-link" :to="{ path: '/' }">
                    <img
                        src="@/assets/buefy.png"
                    >
                </b-navbar-item>
            </template>
            <template slot="start" style="float:left">
                <nuxt-link
                to="/index"
                exact-active-class="is-active"
                tag="b-navbar-item"
                >
                    ホーム
                </nuxt-link>
                <nuxt-link
                :to="`/map`"
                exact-active-class="is-active"
                tag="b-navbar-item"
                >
                    マップ
                </nuxt-link>
                <nuxt-link
                :to="`/post`"
                exact-active-class="is-active"
                tag="b-navbar-item"
                >
                    投稿
                </nuxt-link>
                <nuxt-link
                :to="`/movie`"
                exact-active-class="is-active"
                tag="b-navbar-item"
                >
                    動画
                </nuxt-link>
            </template>
        </b-navbar>
        <nuxt />
    </div>
    
</template>

<script>
export default {
  
}

</script>

<style>

</style>
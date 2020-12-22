<template>
    <section class="section">
        <div id="post" class="container column is-10">
            <Post></Post>
        </div>
    </section>
</template>

<script>
import Post from '~/components/organisms/post.vue'

export default {
    layout:'default',
    components:{
        Post
    }
}
</script>

<style>

</style>
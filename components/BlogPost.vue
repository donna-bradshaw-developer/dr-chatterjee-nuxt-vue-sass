<template>
    <div class="blog-post">
        <div class="blogImage" :style="{ backgroundImage: 'url(' + post.postImage + ')' }">
        </div>

        <div class='blogContent'>
          <h6 class='blogCategory'>{{ post.postCategory }}</h6>
          <h5 class='blogPostTitle'>{{ post.postHeader }}</h5>
          <p class="blogPostBody">{{ post.postBody }}</p>
          <span><nuxt-link to="">{{ post.moreInfo }}</nuxt-link></span>
        </div>
    </div>
</template>

<script>
export default {
    props: ["post", "desktopImage"]
}
</script>

<style>
 
</style>

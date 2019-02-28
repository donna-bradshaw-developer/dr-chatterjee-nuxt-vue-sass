<template>
    <div  class="sideImage" :style="{ backgroundImage: 'url(' + sideMenu + ')' }">
          <img :src='image' />
    </div>
</template>

<script>
export default {
    props: ["sideMenu", 'image']

}
</script>

<style>

</style>

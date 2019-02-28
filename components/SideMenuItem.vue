<template>
    <div  class="sideImage" :style="{ backgroundImage: 'url(' + sideMenu + ')' }">
    </div>
</template>

<script>
export default {
    props: ["sideMenu"]

}
</script>

<style>

</style>

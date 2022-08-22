<template>
    <button :style="{background: colour}" class="btn">{{text}}</button>
</template>

<script>
export default{
    name: "Button",
    props: {
        text: String,
        colour: String,
    }
}
</script>

<style scoped>

</style>

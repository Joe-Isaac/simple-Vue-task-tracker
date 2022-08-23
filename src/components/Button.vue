<template>
    <button @click="onClick()" :style="{background: colour}" class="btn">{{text}}</button>
</template>

<script>
export default{
    name: "Button",
    props: {
        text: String,
        colour: String,
    },
    methods: {
        onClick() {
            console.log("Button clicked")
        }
    }
}
</script>

<style scoped>

</style>

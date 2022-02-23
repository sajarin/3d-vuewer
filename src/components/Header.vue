<script setup>
    defineProps({
        color: {
            type: String,
            default: "black",
            required: false 
        }, 
        text: {
            type: String,
            default: "This is a header",
            required: true
        }
    })
</script>

<template>
    <div class="header" :style="{backgroundColor: color}">
        <h1> {{ text }} </h1>
    </div>
</template>

<style>
    .header {
        width: 75%;
        height: 50px;
        margin:0 auto;
        text-align: center;
    }
    h1 {
        font-size: 2.8rem;
        line-height: 3.2rem;
        font-family: "Garet Heavy";
        letter-spacing: 5px;
        color: white;
    }
</style>
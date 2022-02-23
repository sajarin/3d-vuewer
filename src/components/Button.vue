<script setup>
    defineProps({
        stylings: {
            type: String,
            default: "btn",
            required: true 
        }, 
        text: {
            type: String,
            default: "This is a button",
            required: true
        }
    })
</script>

<template>
    <button :class="stylings"> {{ text }} </button>
</template>

<style scoped>
.btn{
    cursor: pointer;
    outline: none;
    overflow: hidden;
    background: none;
    padding: 12px 24px;
    margin-right: 10px;
    font-family: "Garet Heavy";
    font-size: 15px;
    letter-spacing: 0.05em;
    border: 2px solid #161616;
    border-radius: 5px;
    color: 161616;
    background-color: white;
}
.highlighted{
    color:black;
    font-family: 'Garet Heavy';
    font-weight: bold;
    background-color: #F8C0B6;
    border: none;
}
.ripple {
    position: relative;
    overflow: hidden;
    transform: translate3d(0,0,0);
}
.ripple:after {
    content: "";
    display: block;
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    pointer-events: none;
    background-image: radial-gradient(circle, #161616 10%, transparent 10.01%);
    background-repeat: no-repeat;
    background-position: 50%;
    transform: scale(10, 10);
    opacity: 0;
    transition: transform .5s, opacity 1s;
}

.ripple:active:after {
    transform: scale(0, 0);
    opacity: .3;
    transition: 0s;
}
/* button {
background-color: #F8C0B6;
font-family: "Garet Heavy";
font-size: 1.25em;
border-radius: 1%;
} */
</style>
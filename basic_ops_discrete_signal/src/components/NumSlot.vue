<template>
  <div>
    <input type="number" id="slot" v-bind:value="value">
  </div>
</template>

<script>
export default {
    name: "NumSlot",
    props:{
        value: {
            type: Number,
            required: false
        }
    }
}
</script>

<style scoped>
    #slot{
        width: 2em;
        margin: 1px;
    }

    input::-webkit-outer-spin-button,
    input::-webkit-inner-spin-button {
        -webkit-appearance: none;
        margin: 0;
    }
</style>
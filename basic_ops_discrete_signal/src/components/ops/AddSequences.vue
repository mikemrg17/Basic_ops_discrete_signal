<template>
    <div>
        Sumar/Restar
        <p>xseq={{xseq}}</p>
        <p>hseq={{hseq}}</p>
        Suma:
    </div>
</template>

<script>
export default {
    name: "AddSequences",
    data(){
        return {
            Add: [],
            Sub: []
        }
    },
    props:{
        xseq: Array,
        hseq: Array
    },
    computed:{

    }
}
</script>

<style>

</style>
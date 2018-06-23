<template>
    <div id="app">
        <PairUp/>
    </div>
</template>

<script>
    import PairUp from './components/PairUp.vue'

    export default {
        name: 'app',
        components: {
            PairUp
        }
    }
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
</style>

<template>
    <div>
        <div v-if="loading">
            <img src="https://loading.io/spinners/default/index.circle-dot-preloader.svg" alt="">
        </div>
    </div>
</template>

<script>
    export default {
        props: ['loading']
    }
</script>
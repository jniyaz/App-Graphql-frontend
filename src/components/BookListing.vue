<template>
    <div>
        <img :src="book.image" alt="cover-image" class="h-64 mb-2">
        <div>
            <router-link class="font-bold text-lg text-black hover:text-grey-darkest" :to="`books/${book.id}`">{{ book.title }}</router-link>
            <div class="text-grey-darkest text-sm"><small>{{ book.author_name }}</small></div>
        </div>
    </div>
</template>

<script>
    export default {
        props: ['book']
    }
</script>
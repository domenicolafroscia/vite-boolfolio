<script>
import { store } from '../store';

export default {
    props: {
        project: Object
    },
    data() {
        return {
            store
        }
    },
    computed: {
        truncateText() {
            if (this.project.content && this.project.content.length > 150) {
                return this.project.content.substring(0, 150) + "...";
            }
            return this.project.content;
        },
    },
}
</script>

<template>
    <div class="card h-100">
        <img :src="`${store.baseUrl}/storage/${project.cover_image}`" alt="" />
        <div class="card-body">
            <h5>{{ project.title }}</h5>
            <span v-for="technologies in project.technologies" :key="technologies.id"> <span><strong>Technology:</strong> {{ technologies.name }}</span> </span>
            <p class="my-2">{{ truncateText }}</p>

            <router-link class="btn btn-primary" :to="{ name: 'single-project', params: {slug: project.slug} }">Details</router-link>
        </div>
    </div>
</template>

<style lang="scss" scoped></style>
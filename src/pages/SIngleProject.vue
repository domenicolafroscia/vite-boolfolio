<script>
import axios from 'axios';
import { store } from '../store';

export default {
    data() {
        return {
            store,
            curProject: null,
            loading: false,
        };
    },
    created() {
        this.loading = true;
        axios
            .get(`${this.store.baseUrl}/api/projects/${this.$route.params.slug}`)
            .then((resp) => {
                if (resp.data.success) {
                    this.curProject = resp.data.result;
                } else {
                    this.$router.push({ name: 'not-found' })
                }
            })
            .finally(() => {
                this.loading = false;
            });
    },
    computed: {
        truncateText() {
            if (this.curProject.content && this.curProject.content.length > 150) {
                return this.curProject.content.substring(0, 150) + "...";
            }
            return this.project.content;
        },
    },
};
</script>

<template>
    <div class="container mt-4">

        <div v-if="loading">
            <h3>Loading...</h3>
        </div>
        <div v-else>
            <h1 class="text-center">Single Project</h1>
            <div class="card h-100">
                <img :src="`${store.baseUrl}/storage/${curProject.cover_image}`" alt="" />
                <div class="card-body">
                    <h5>{{ curProject.title }}</h5>
                    <span v-for="technologies in curProject.technologies" :key="technologies.id">
                        <span><strong>Technology:</strong> {{ technologies.name }}</span> </span>
                    <p class="my-2">{{ truncateText }}</p>
                </div>
            </div>
        </div>

    </div>
</template>

<style scoped lang="scss"></style>
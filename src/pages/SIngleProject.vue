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
                this.curProject = resp.data.result;
            })
            .finally(() => {
                this.loading = false;
            });
    },
};
</script>

<template>
    <div class="container mt-4">

        <div v-if="loading">
            <h3>Loading...</h3>
        </div>
        <div v-else>
            <h1 class="text-center">{{ curProject.title }}</h1>
        </div>

    </div>
</template>

<style scoped lang="scss"></style>
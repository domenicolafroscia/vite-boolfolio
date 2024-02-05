<script>
import axios from 'axios';
import ProjectCard from '../components/ProjectCard.vue';
import { store } from '../store';

export default {
  data() {
    return {
      store,
      projects: [],
      totalPage: 0,
      curPage: 1,
      lastPage: 1
    }
  },
  created() {
    this.getProjects(1);
  },
  components: { ProjectCard },
  methods: {
    getProjects(pages) {
      this.curPage = pages;
      axios
        .get(`${this.store.baseUrl}/api/projects`, {
          params: {
            page: pages,
          }
        })
        .then((resp) => {
          this.projects = resp.data.result.data;
          this.lastPage = resp.data.result.last_page;
          this.totalPage = resp.data.result.total;
        })
    }
  },
}
</script>

<template>
  <div class="container">
    <h2 class="text-center">Project Card</h2>

    <div class="row row-cols-1 row-cols-md-3 row-cols-lg-4 g-3">
      <div class="col" v-for="project in projects" :key="project.id">
        <ProjectCard :project="project" />
      </div>
    </div>

    <div class="mt-4">
      <nav aria-label="Paginate">
        <ul class="pagination">
          <li class="page-item" :class="{ 'disabled': curPage === 1 }">
            <a class="page-link" href="#" @click.prevent="getProjects(curPage - 1)">Previous</a>
          </li>

          <li class="page-item" aria-current="page" v-for="numPage in lastPage" :class="{ 'active': numPage === curPage }">
            <a class="page-link" href="#" @click.prevent="getProjects(numPage)">{{ numPage }}</a>
          </li>

          <li class="page-item" :class="{ 'disabled': curPage === lastPage }">
            <a class="page-link" href="#" @click.prevent="getProjects(curPage + 1)">Next</a>
          </li>
        </ul>
      </nav>
    </div>

  </div>
</template>

<style lang="scss" scoped></style>
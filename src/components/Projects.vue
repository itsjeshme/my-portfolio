<template>
  <section id="projects" class="container py-5">
    
    <h1 class="text-center mb-5">
      Projects
    </h1>

    <div class="container">

      <div
        v-for="(group, index) in chunkedProjects"
        :key="index"
        class="row justify-content-center mb-4"
      >

        <ProjectCard
          v-for="project in group"
          :key="project.id"
          :project="project"
        />

      </div>

    </div>
  </section>
</template>

<script setup>
	import {computed} from 'vue';

	import ProjectCard from './ProjectCard.vue';

	import projects from '../data/projects.json';

	const chunkSize = 3;

	// this function is responsible for splitting the projects array into smaller arrays (chunks) with 3 chunkSize (elements) per array.
	const chunkedProjects = computed(() => {
		const chunks = [];

		for(let i = 0; i < projects.length; i += chunkSize){
			chunks.push(projects.slice(i, i + chunkSize));
		}

		return chunks;
	})
</script>
<template>
  <div class="col-12 col-md-4 col-lg-4 text-center">

    <img
      :src="project.image"
      :alt="project.title"
      class="img-fluid p-5"
    >

    <h5>
      {{ project.title }}
    </h5>

    <p>
      {{ project.description }}
    </p>

  </div>
</template>

<script setup>
	import {defineProps} from 'vue';

	defineProps({
		project: Object
	})
</script>
<script setup>

  import Navbar from './components/Navbar.vue';
  import Landing from './components/Landing.vue';
  import WorkExperience from "./components/WorkExperience.vue";
  import Projects from "./components/Projects.vue";
  import Tools from "./components/Tools.vue";
  import Affiliations from "./components/Affiliations.vue";
  import Contact from "./components/Contact.vue";
  import FooterComponent from "./components/FooterComponent.vue";
  
</script>

<template>

    <Navbar />

    <main>
      <Landing />
      <WorkExperience />
      <Projects />
      <Tools />
      <Affiliations />
      <Contact />
    </main>

    <FooterComponent />

</template>

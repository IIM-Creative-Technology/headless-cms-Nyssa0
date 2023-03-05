<template>
  <section>
    <div class="container">
      <Nav/>
      <div class="content" v-if="project">
        <div class="content__banner">
          <div class="content__banner__intro cbi__project">
            <h1 v-if="project.name">{{ project.name }}</h1>
            <h3 v-if="project.date">{{ project.date }}</h3>
            <p v-if="project.description">{{ project.description }}</p>
            <div v-if="project.link || project.github" class="content__banner__intro__links">
              <nuxt-link :to="project.link" class="project__links">
                Site
              </nuxt-link>
              <nuxt-link v-if="project.github" :to="project.github" class="project__links">
                Github
              </nuxt-link>
            </div>
          </div>
<!--          <div v-if="hero.data.Hero.Image" class="memoji">-->
<!--            <img :src="hero.data.Hero.Image.url" alt="">-->
<!--          </div>-->
        </div>
        <div class="project__container">
          <video v-if="project.video" width="560" height="315" controls>
            <source :src="project.video.url" type="video/mp4">
          </video>
          <div v-if="project.technologies" class="project__container__icons">
            <ul>
              <li v-for="technology in project.technologies">
                <div class="project__container__icons__box"><img :src="technology.image.url" alt=""></div>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </section>


</template>

<script setup>
import Nav from "../../layouts/nav";

  const { findOne } = useStrapi()
  const route = useRoute()
  const project = ref()

  onMounted(async () => {
    project.value = await findOne(`projects?filters[slug]=${route.params.slug}&populate=deep`)
    project.value = project.value.data[0]

  })
</script>

<style>
@import "../style/main.css";
  .project__container__icons__box img {
    width: 4rem;
  }

</style>
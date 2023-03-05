<template>
  <section>
    <div class="container ctn__projets">
      <Nav/>
      <div class="content">
        <div class="content__banner" v-if="hero">
          <div class="content__banner__intro cbi__etudes">
            <h1 v-if="hero.data.Hero.Title">{{ hero.data.Hero.Title }}</h1>
            <p v-if="hero.data.Hero.Description">{{ hero.data.Hero.Description }}</p>
            <div class="content__banner__intro__links" v-if="types">
              <div v-if="hero.data.Hero.Link1" class="btn"><a class="link" :href="hero.data.Hero.Link1" target="_blank">Github</a></div>
              <div class="btn"><a class="link" @click="filterProjects('all')">reset</a></div>
              <div class="btn" v-for="type in types">
                <a :class="{'link': true, 'is-active': activeFilter === type}" @click="filterProjects(type)">{{ type }}</a>
              </div>
            </div>
          </div>
          <div v-if="hero.data.Hero.Image" class="memoji">
            <img :src="hero.data.Hero.Image.url" alt="">
          </div>
        </div>
        <div class="card__container" v-if="projects">
          <div class="card__container__tiltCard" v-for="project in filteredProjects" :key="project.id">
            <div class="card__container__tiltCard__content">
              <div v-if="project.name" class="card__container__tiltCard__content__title">
                <h2>{{ project.name }}</h2>
              </div>
              <div v-if="project.image" class="card__container__tiltCard__content__img">
                <img :src="project.image.url" alt="">
              </div>
              <div v-if="project.introduction" class="card__container__tiltCard__content__desc">
                <p>{{ project.introduction }}</p>
              </div>
              <div class="card__container__tiltCard__content__link">
                <nuxt-link :to="`/project/${project.slug}`">
                  Voir plus
                </nuxt-link>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import Nav from "../layouts/nav";

const { find } = useStrapi()

const projects = ref()
const technologies = ref()
const types = ref([])
const activeFilter = ref('all')
const hero = ref()

const filterProjects = (type) => {
  activeFilter.value = type
}

const filteredProjects = computed(() => {
  if (activeFilter.value === 'all') return projects.value.data
  return projects.value.data.filter(project => project.type === activeFilter.value)
})

onMounted( async () => {
  projects.value = await find('projects',
      {
        populate: 'deep',
        // filters: { type: test },
        sort: { date: 'DESC'}

      })
  technologies.value = await find('technologies', { populate: 'deep'})
  types.value = new Set(projects.value.data.map( project => {
    return project.type
  }))
  hero.value = await find('projects-page', { populate: 'deep'})

})

</script>

<style lang="css">
@import "style/main.css";
</style>
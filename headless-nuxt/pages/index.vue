<template>
  <section>
    <div class="container" v-if="hero">
      <Nav/>
      <div class="content__banner__intro cbi__index">
        <h1 v-if="hero.data.Hero.Title">{{ hero.data.Hero.Title }}</h1>
        <p v-if="hero.data.Hero.Description">{{ hero.data.Hero.Description }}</p>
        <div v-if="hero.data.Hero.Link1 || hero.data.Hero.Link2" class="content__banner__intro__links">
          <div v-if="hero.data.Hero.Link1" class="btn"><a class="link" :href="hero.data.Hero.Link1" target="_blank">Linkedin</a></div>
          <div v-if="hero.data.Hero.Link2" class="btn"><a class="link" :href="hero.data.Hero.Link2" target="_blank">Github</a></div>
        </div>
      </div>
      <div v-if="hero.data.Hero.Image" class="memoji">
        <img :src="hero.data.Hero.Image.url" alt="">
      </div>
      <div class="copyright">
        <p>
          hassinaayachi99@gmail.com
        </p>
        <p>
          06 37 65 76 45
        </p>
        <p>
          ©Hassina Ayachi
        </p>
      </div>
    </div>
  </section>
</template>

<script setup>
import Nav from "../layouts/nav";

const { find } = useStrapi()

const hero = ref()

onMounted( async () => {
  hero.value = await find('homepage', { populate: 'deep'})
})
</script>

<style lang="css">
@import "style/main.css";
</style>
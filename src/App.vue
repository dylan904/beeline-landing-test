<template>
  <div id="app">
    <main>
      <Navigation />
      <Carousel />
      <LandingSearch />
    </main>
  </div>
</template>

<script>
import Navigation from "./components/Navigation";
import Carousel from "./components/Carousel";
import LandingSearch from "./components/LandingSearch";

export default {
  name: "app",
  components: { Navigation, Carousel, LandingSearch }
};
</script>

<style src="./assets/styles/App.css"/>



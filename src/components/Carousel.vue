<template>
  <div id="slider">
    <transition-group tag="div" name="parent-slide" class="slides-group">
      <div v-if="show" :key="current" class="slide">
        
        <transition name="fade-slide" appear>
          <div :class="'slide-bg slide-from-' + slides[current].slideFrom" v-bind:style="bgImage"></div>
        </transition>

        <div class="slide-wrap">
          <div class="slide-content" ref="slide">
            
            <transition name="slide-header-down" appear>
              <div class="slide-header" v-if="show">
                <h3 class="slide-country">{{slides[current].country}}</h3>
                <h1 class="slide-title">{{slides[current].title}}</h1>
              </div>
            </transition>

            <div class="slide-bottom" ref="slideBottom">
              <nav class="slide-nav">
                <div class="btn btn-prev" aria-label="Previous slide" @click="slide(-1)">&#10094;</div>
                <span class="slide-nav-status">
                  <b class="slide-nav-current">{{"0" + (current+1)}}</b>
                  <p class="slide-nav-of">{{"&nbsp;/ 0" + slides.length}}</p>
                </span>

                <div class="btn btn-next" aria-label="Next slide" @click="slide(1)">&#10095;</div>
              </nav>

              <div class="slide-forecast-wrap">
                <div class="slide-forecast">
                  <i :class="'slide-forecast-icon fas fa-' + slides[current].icon"></i>
                  <span class="slide-forecast-temp">{{slides[current].temp}}&deg;</span>
                  <span class="slide-forecast-text">{{slides[current].weather}}</span>
                </div>
              </div>
            </div>
          </div>
        </div>
        
      </div>
    </transition-group>
  </div>
</template>

<script>
export default {
  name: "Carousel",
  data() {
    return {
      current: 0,
      direction: 1,
      show: false,
      showContent: false,
      slides: [
        { className: "blue", country: "Iceland", title: "Mountain Escape", weather: "N/A", temp: "--", city: "Reykjavik", slideFrom: "right" },
        { className: "red", country: "Japan", title: "Future Cityscape", weather: "N/A", temp: "--", city: "Tokyo", slideFrom: "left" },
        { className: "yellow", country: "Laos", title: "Fancy Waterfall", weather: "N/A", temp: "--", city: "Vientiane", slideFrom: "top" }
      ]
    }
  },
  methods: {
    slide(dir) {
      const lastSlideWidth = this.$refs.slide.clientWidth;
      setTimeout(() => {
        const nextSlideWidth = this.$refs.slide.clientWidth;
        this.$refs.slideBottom.style.width = (lastSlideWidth) ? lastSlideWidth + 'px' : '100%';
        this.$refs.slideBottom.style.transition = 'none';

        setTimeout(() => {
          this.$refs.slideBottom.style.width = (nextSlideWidth) ? nextSlideWidth + 'px' : '100%';
          this.$refs.slideBottom.style.transition = 'width 1s ease-out';
        }, 10)
      }, 0)
      
      this.direction = dir;
      var len = this.slides.length;
      this.current = (this.current + dir % len + len) % len;
    }
  },
  created() {
    const iconMap = {
      "01": ["sun", "moon"], 
      "02": ["cloud-sun", "cloud-moon"], 
      "03": ["cloud", "cloud"], 
      "04": ["cloud-meatball", "cloud-meatball"], 
      "09": ["cloud-rain", "cloud-rain"],
      "10": ["cloud-sun-rain", "cloud-moon-rain"], 
      "11": ["poo-storm", "poo-storm"], 
      "13": ["snowflake", "snowflake"], 
      "50": ["smog", "smog"]
    };

    const time = new Date().getTime() / 1000;

    for (let slide of this.slides) {
      fetch("https://api.openweathermap.org/data/2.5/weather?q=" + slide.city + "&units=imperial&appid=44871278a815cf66cf4cbf3ebce7f2c5")
      .then(response => response.json())
      .then(data => {
        slide.weather = data.weather[0].description;
        slide.temp = parseInt(data.main.temp);
        const isNight = (time < data.sys.sunrise || time > data.sys.sunset) ? 1 : 0;
        slide.icon = iconMap[data.weather[0].icon.substr(0, 2)][isNight];
      });
    }
  },
  mounted() {
    this.show = true;
    setInterval((() => { this.slide(1) }).bind(this), 500000)
  },

  computed: {
    bgImage() {
      return {
        backgroundImage: `url(${require('../assets/images/' + this.slides[this.current].country + '.jpg')})`
      };
    }
  }
};
</script>


<style src="../assets/styles/Carousel.css" scoped></style>
<style src="../assets/styles/Slide.css" scoped></style>
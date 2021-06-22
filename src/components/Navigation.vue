<template>
  <div class="nav" id="nav">
    <div class="nav-wrap">
      <div class="nav-header">
        <a href="#" class="nav-title-link">
          <img class="nav-title" src="https://devbydylan.com/beeline.svg" />
        </a>
      </div>

      <input type="checkbox" id="nav-check" />
      <div class="nav-btn">
        <label for="nav-check">
          <span class="nav-check-line"></span>
          <span class="nav-check-line"></span>
          <span class="nav-check-line"></span>
        </label>
      </div>

      <div class="nav-links-wrap">
        <div class="nav-links nav-links-left">
          <a class="nav-link" href="#">Deal</a>
          <a class="nav-link" href="#">Flight</a>
          <a class="nav-link" href="#">Discovers</a>
          <a class="nav-link" href="#">News</a>
        </div>

        <div class="nav-links nav-links-right">
            <span class="nav-link select-link" >
                <Select
                    name="lang"
                    :opts="[
                      { name: 'en', text: 'English' },
                      { name: 'es', text: 'Español' },
                      { name: 'fr', text: 'François' },
                    ]"
                />
            </span>

          <a class="nav-link" href="#">Sign up</a>
          <a class="nav-link" href="#">Log in</a>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Select from "./sub/Select.vue";

export default {
  components: { Select },
  name: "Navigation",
};
</script>


<style src="../assets/styles/Navigation.css" scoped></style>
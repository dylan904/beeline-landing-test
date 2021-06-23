<template>
    <div class="container" :class="{ expanded: active }" 
        @mouseenter="hover = true" @mouseleave="hover = false" @click="forceFocus($event)">
        <div class="wrap">
            <span class="label-wrap">
                <label :for="'input-' + name">
                    <i :class="'input-icon fas fa-' + icon"></i>
                    {{ text }}
                </label>
            </span>
            <span class="input-wrap">
                <Autocomplete v-if="autocomplete"
                    :onChange="update" :onFocus="onFocus" :onBlur="onBlur" />
                <input v-else :type="type" :name="'input-' + name" tabindex="0" :min="(type === 'date') ? min : false" :data-min="min"
                    @change="update($event)" @focus="onFocus()" @blur="onBlur()" />
            </span>
        </div>
    </div>
</template>

<script>
import Autocomplete from "./Autocomplete.vue";

export default {
  name: "LandingInput",
  components: {Autocomplete},
  props: {
    name: {name: String, required: true}, 
    type: {name: String, required: true}, 
    text: {name: String, required: true},
    icon: {name: String, required: true},
    autocomplete: Boolean,
    onChange: Function,
    min: String
  },

  data() {
    return {
      active: false,
      value: "",
      clicked: false
    };
  },

  created() {
    console.log('created', this.name, this.min)
  },

  methods: {
    forceFocus(ev) {
      const target = ev.currentTarget;
      this.active = true;
      setTimeout(() => {
        target.querySelector("input").focus();
      }, 0);
    },
    update(ev) {
      this.value = ev.target.value;
      this.onChange(this.name, this.value);
    },
    onFocus() {
        if (!this.active) {
            this.active = true;
        }
    },
    onBlur() {
      if (!this.value.length && !this.hover) {
        this.active = false;
      }
    },
  },
};
</script>

<style src="../../assets/styles/LandingInput.css" scoped></style>
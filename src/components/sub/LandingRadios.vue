<template>
    <div class="radio-container">
        <template v-for="(opt, x) in opts">
            <span :key="x" class="radio-option-wrap">
                <input type="radio" :id="name + '-opt-' + x" name="radio-group" :checked="current === x" @change="onSelect(x)">
                <label :for="name + '-opt-' + x">{{opt.text}}</label>
            </span>
        </template>
    </div>
</template>

<script>
    export default {
        name: "LandingRadios",
        props: {
            name: {type: String, required: true},
            opts: {type: Array, required: true},
            onChange: {type: Function, required: true},
        },
        
        data() {
            return {
                current: 0
            }
        },

        created() {
            this.onChange(this.opts[0].name);
        },

        methods: {
            onSelect(idx) {
                this.current = idx;
                //console.log('trigger 1', this.onSelect)
                this.onChange(this.opts[idx].name);
            }
        }
    };
</script>

<style src="../../assets/styles/LandingRadios.css" scoped></style>
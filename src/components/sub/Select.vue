<template>
    <span class="dropdown-el" :class="{ expanded: hover }"  v-on:tap="test($event)"
            @mouseover="hover = true" @mouseleave="hover = false" @scroll="blockScroll($event)">
        <template v-for="(opt, x) in opts">
            <input :key="x + '-i'" type="radio" :id="name + '-' + opt.name" @focus="blockFocus($event)"
                :name="'select-' + name" :value="opt.name" :checked="current === x">
            <label :key="x + '-l'" :for="name + '-' + opt.name" @click="onChange(x)" v-on:tap="hover = false">{{opt.text}}</label>
        </template>
    </span>
</template>

<script>
    export default {
        name: "Select",
        props: {
            name: String,
            opts: Array,
            onSelect: Function
        },
        
        data() {
            return {
                current: 0,
                hover: false
            }
        },

        methods: {
            blockFocus(ev) {
                console.log('focus', ev.target, ev.target.labels[0], ev.target.parentElement.parentElement.querySelector(':focus'));
                ev.target.blur();
            },
            blockScroll(ev) {
                ev.target.scrollTop = 0;
            }, 
            test(ev) {
                console.log('tap', ev.target, ev.currentTarget)
            },
            onChange(idx) {
                this.hover = false;
                this.current = idx;
                console.log('trigger 1', this.onSelect)
                if (typeof this.onSelect === 'function') {
                    this.onSelect(this.opts[idx].name);
                }
            }
        }
    };
</script>

<style src="../../assets/styles/Select.css" scoped></style>
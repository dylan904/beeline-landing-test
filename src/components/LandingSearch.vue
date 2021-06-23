<template>
    <div class="search-container">
        <LandingRadios name="destination" :opts="destinationTypeOpts" :onChange="destinationTypeChange" />

        <div class="search-wrap">
            <template v-for="(opt, x) in searchOpts">
                <LandingInput :key="x" :name="opt.name" :type="opt.type" :text="opt.text" :icon="opt.icon" :autocomplete="opt.autocomplete" :onChange="changeSearchOpt" :min="opt.min" />
            </template>

            <button type="submit" class="search-submit">
                <span class="search-submit__mask"></span>
                <span class="search-submit__text">SEARCH</span>
                <span class="search-submit__text search-submit__text--bis">SEARCH</span>
            </button>
        </div>
    </div>
</template>

<script>
    import LandingInput from "./sub/LandingInput.vue";
    import LandingRadios from "./sub/LandingRadios.vue";

    export default {
        name: "LandingSearch",
        components: { LandingInput, LandingRadios },
        
        data() {
            return {
                searchOpts: [
                    {name: 'location', type: 'text', text: 'Where are you going?', icon: 'map-marker-alt', autocomplete: true},
                    {name: 'checkin', type: 'date', text: 'Check In', icon: 'calendar', min: new Date().toISOString().split("T")[0]},
                    {name: 'checkout', type: 'date', text: 'Check Out', icon: 'calendar', min: new Date().toISOString().split("T")[0]},
                    {name: 'travler-count', type: 'number', text: 'Number of Travelers', icon: 'users'}
                ],
                searchData: {},
                destinationTypeOpts: [
                    {name: 'places', text: 'Places to Stay'},
                    {name: 'adventures', text: 'Adventures'}
                ]
            }
        },

        methods: {
            destinationTypeChange(name) {
                this.destinationType = name;
                console.log('test', this.searchOpts[1].min)
                this.logCurrentSearch();
            },
            changeSearchOpt(name, value) {
                this.searchData[name] = value;

                if (name === 'checkin') {
                    this.searchOpts.find(opt => opt.name === 'checkout').min = value
                }

                this.logCurrentSearch();
            },
            logCurrentSearch() {
                console.group('Current Search');
                console.log('Search Destination', this.destinationType);
                console.log('Search Data', this.searchData);
                console.groupEnd('Current Search');
            }
        }
    };
</script>

<style src="../assets/styles/LandingSearch.css" scoped></style>
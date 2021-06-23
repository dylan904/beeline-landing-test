<template>
  <div class="autocompleteel">
    <div>
      <input type="text" @keyup="loadSuggestions"
        placeholder="Enter some text" v-model="searchText"
        @change="onChange($event)" @focus="onFocus()" @blur="onBlur()"
      />
      <br />
      <div class="suggestionlist" v-if="suggestions.length">
        <ul>
          <li v-for="(result, idx) in suggestions" :key="idx"
            class="typeahead-result-item"
            @click="itemSelected(idx)"
          >
            <button>
                <i :class="'result-icon fas fa-' + iconMap[result.type]"></i>

                <div class="typeahead-button-label">
                    <div class="truncate">
                        <span>
                            <strong>{{result.regionNames.primaryDisplayName}}</strong>
                        </span>
                    </div>
                    <div class="is-subText truncate">{{result.regionNames.secondaryDisplayName}}</div>
                </div>

            </button>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Autocomplete",
  props: {
      onChange: {type: Function, required: true},
      onFocus: Function,
      onBlur: Function
  },

  data() {
    return {
        searchText: '',
        suggestions: [],
        iconMap: {
            AIRPORT: 'plane-arrival',
            CITY: 'city',
            HOTEL: 'hotel',
            METROCODE: 'city',
            MULTICITY: 'city',
            MULTIREGION: 'city',
            NEIGHBORHOOD: 'city',
            POI: 'map-marker-alt',
            TRAINSTATION: 'train'
        }
    };
  },

  methods: {
    loadSuggestions: function(){
        var el = this;
        this.suggestions = [];

        if (this.searchText.length) {
            fetch('https://www.expedia.com/api/v4/typeahead/' + encodeURIComponent(this.searchText) + '?dest=true&features=consistent_display&format=json&guid=3a3945bf-8f10-41e2-8633-43559b1e57ae&lob=PACKAGES&locale=en_US&maxresults=5&personalize=true&regiontype=991')
            .then(response => response.json())
            .then(data => {
                el.suggestions = data.sr;
                console.log(el.suggestions[0].regionNames.shortName)
            });
        }
    },

      itemSelected: function(i){
         var id = this.suggestions[i].regionNames.primaryDisplayName; 
         var name = this.suggestions[i].regionNames.primaryDisplayName;

         this.searchText = name;
         

         console.log('emit', this.suggestions[i].regionNames.primaryDisplayName, this.suggestions[i])

         this.$emit("input", id);

         this.suggestions = [];
      }
  },
};
</script>

<style src="../../assets/styles/Autocomplete.css" scoped></style>
<template>
  <v-container>
    <v-row class="text-center">
      <v-col class="mb-4">
        <h1 class="display-2 font-weight-bold mb-3">
          Welcome to Countries Infos
        </h1>
      </v-col>
      <v-col cols="12">
        <v-card
          color="red lighten-2"
          dark
        >
          <v-card-title class="headline red lighten-3">
            Search for Country
          </v-card-title>
          <v-card-text>
            Explore all the Countries in the world.
          </v-card-text>
          <v-card-text>
            <v-autocomplete
              v-model="model"
              :items="items"
              :loading="isLoading"
              :search-input.sync="search"
              color="white"
              hide-no-data
              hide-selected
              item-text="Description"
              item-value="API"
              label="Countries"
              placeholder="Start typing to Search"
              prepend-icon="mdi-flag"
              return-object
            ></v-autocomplete>
          </v-card-text>
          <v-divider></v-divider>
          <v-expand-transition>
            <v-list v-if="model" class="red lighten-3">
              <v-list-item
                v-for="(field, i) in fields"
                :key="i"
              >
                <v-list-item-content>
                  <v-list-item-title v-text="field.value"></v-list-item-title>
                  <v-list-item-subtitle v-text="field.key"></v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
            </v-list>
          </v-expand-transition>
          <v-card-actions>
            <v-radio-group v-model="selectedCategory" row>
              <v-radio model="selectedCategory" label="Name" value="name"></v-radio>
              <v-radio model="selectedCategory" label="Capital" value="capital"></v-radio>
              <v-radio model="selectedCategory" label="Region" value="region"></v-radio>
            </v-radio-group>
            <v-spacer></v-spacer>
            <v-btn
              :disabled="!model"
              color="grey darken-3"
              @click="model = null"
            >
              Clear
              <v-icon right>mdi-close-circle</v-icon>
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
      <v-col>
        <v-container fluid>
          <v-row dense>
            <v-col
              v-if="isLoading"
            >
            <v-progress-circular
              indeterminate
              color="red"
            ></v-progress-circular>
            </v-col>
            <v-col
              v-else
              v-for="country in filteredCountry"
              :key="country.name"
              cols="6"
              v-bind:pagination.sync="pagination"
              v-bind:items="countries"
              v-bind:search="search"
            >
              <v-card>
                <v-img
                  :src="country.flag"
                  class="white--text align-end"
                  gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,.5)"
                  height="200px"
                >
                  <v-card-title v-text="country.name"></v-card-title>
                </v-img>
                <v-card-subtitle class="pb-0">Capital: {{country.capital}}</v-card-subtitle>
                <v-card-subtitle class="pb-0">Region: {{country.region}}</v-card-subtitle>

                <v-card-actions>
                  <v-spacer></v-spacer>

                  <v-btn
                    dark
                    @click.stop="dialog = true; country_select=country"
                  >
                    Read More
                  </v-btn>

                </v-card-actions>
              </v-card>
            </v-col>
          </v-row>
        </v-container>
      </v-col>
    </v-row>
    <v-row class="text-center">
      <v-container class="max-width">
        <v-pagination
          v-model="pagination.page"
          :length="pages"
        ></v-pagination>
      </v-container>
    </v-row>
    <v-dialog
      v-model="dialog"
      max-width="1200"
    >
      <v-card>
        <v-card-title class="headline">{{country_select.name}}</v-card-title>
        <v-card-text>
          <h3>
            Currencies
          </h3>
          </v-card-text>
          <v-card-text v-for="(currency, i) in country_select.currencies" :key="i">
            {{currency.name}}
          </v-card-text>

          <v-card-text>
          <h3>
            Currencies
          </h3>
          </v-card-text>
          <v-card-text v-for="(currency, i) in country_select.currencies" :key="i">
            {{currency.name}}
          </v-card-text>

          <v-card-text>
            <h3>
              Border Country
            </h3>
          </v-card-text>
          <v-card-text v-for="(border, i) in country_select.borders" :key="i">
            <v-btn
              color="green darken-1"
              text
              @click="changeDialog(border)"
            >
            {{border}}
          </v-btn>
          </v-card-text>

          <v-card-text>
            <h3>
              Alt Spelling
            </h3>
          </v-card-text>
          <v-card-text v-for="(altSpelling, i) in country_select.altSpellings" :key="i">
            {{altSpelling}}
          </v-card-text>

          <!-- Afficher Google maps -->

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn
            color="green darken-1"
            text
            @click="dialog = false"
          >
            Close
          </v-btn>

        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
  export default {
    name: 'CountriesSearch',

    data: () => ({
      selectedCategory: 'all',
      descriptionLimit: 60,
      dialog: false,
      entries: [],
      isLoading: false,
      model: null,
      search: null,
      countries: [],
      country_select: {},
      all_countries: [],
      pagination: {
        page: 1,
        total: 0,
        perPage: 20,
        visible: 7
      },
    }
  ),
  methods:{
    changeDialog(val) {
      for (let index = 0; index < this.countries.length; index++) {
        const element = this.countries[index];
        if (element.cioc == val)
          this.country_select = element
      }
    },
    init(){
      if (this.isLoading) return
      this.isLoading = true
      fetch(`https://restcountries.eu/rest/v2/all`)
        .then(res => res.json())
        .then(res => {
          this.countries = res
        })
        .catch(err => {
          console.log(err)
        })
        .finally(() => {
          this.isLoading = false
      })
    }
  },
  created(){
    this.init()
  },
  computed: {
      fields () {
        if (!this.model) return []

        return Object.keys(this.model).map(key => {
          return {
            key,
            value: this.model[key] || 'n/a',
          }
        })
      },
      items () {
        return this.entries.map(entry => {
          const Description = entry.Description.length > this.descriptionLimit
            ? entry.Description.slice(0, this.descriptionLimit) + '...'
            : entry.Description

          return Object.assign({}, entry, { Description })
        })
      },
      pages () {
        return this.pagination.rowsPerPage ? Math.ceil(this.countries.length / this.pagination.rowsPerPage) : 0
      },
      filteredCountry: function() {
        var vm = this;
        var category = vm.selectedCategory;
        
        if(category == "all") {
          return this.countries;
        } else {
          if (category == 'name'){
            return vm.sort(function(a, b) {
              if(a.name < b.name) { return -1; }
              if(a.name > b.name) { return 1; }
              return 0;
            });
          } else if (category == 'capital') {
            return vm.countries.sort(function(a, b) {
              if(a.capital < b.capital) { return -1; }
              if(a.capital > b.capital) { return 1; }
              return 0;
            });
          } else if (category == 'region'){
            return vm.countries.sort(function(a, b) {
              if(a.region < b.region) { return -1; }
              if(a.region > b.region) { return 1; }
              return 0;
            });
          }
          return vm.countries
        }
      }
    },

    watch: {
      search (val) {
        console.log(val)
        // Items have already been loaded
        if (this.items.length > 0) return

        // Items have already been requested
        if (this.isLoading) return

        this.isLoading = true

        // Lazily load input items
        fetch(`https://restcountries.eu/rest/v2/name/${val}`)
          .then(res => res.json())
          .then(res => {
            this.countries = res
          })
          .catch(err => {
            console.log(err)
          })
          .finally(() => (this.isLoading = false))
      },
    },
  }
</script>

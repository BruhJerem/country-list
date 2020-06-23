<template>
  <div class="home">
    <CountriesSearch/>
  </div>
</template>

<script>
// @ is an alias to /src
import CountriesSearch from '@/components/CountriesSearch.vue'

export default {
  name: 'Home',
  components: {
    CountriesSearch
  }
}
</script>

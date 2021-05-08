<template>
  <div class="home">
      <div class='weather'>
          <Weather></Weather>
      </div>
      <div class='calander'>
          <Calander></Calander>
      </div>
  </div>
</template>

<script>
// @ is an alias to /src

import Weather from '@/components/Weather.vue'
import Calander from '@/components/Calander.vue'
export default {
  name: 'Home',
  components: {
    Weather,
    Calander
  }
}
</script>

<template>
  <main-comp />
</template>

<script>
import MainComp from '@/components/MainComp.vue'
// import { mapState } from 'vuex'

export default {
  name: 'HomeView',
  components: {
    MainComp
  },
  computed: {
    // ...mapState([
    //   'titulo'
    // ])
  }
}
</script>

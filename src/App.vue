<script>
import SongList from './components/SongList.vue';

export default{
  name:'App',
  components:{
    SongList
  }
}
</script>


<template>
  <!-- Use the project's player layout for a responsive, techy look -->
  <div class="player-app">
    <SongList/>
  </div>
</template>

<style>
/* keep App.vue light — main layout lives in src/components/style.css */
#app{font-family:Avenir,Arial, Helvetica, sans-serif}
</style>
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
  <div class="app-container p-3 bg-black m-auto">
    <SongList/>
  </div>
</template>

<style>
.app-container{
  width:350px;
  height:700px;
}

#app{
  font-family:Avenir,Arial, Helvetica, sans-serif;
  -webkit-font-smoothing:antialiased;
  -moz-osx-font-smoothing:graycale;
  margin-top:20px;
}

</style>
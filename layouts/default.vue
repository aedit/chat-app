<template>
  <div class="def-layout" >
    <TopBar/>
    <SideBar/>
    <Nuxt class="main bg-gray-100" />
  </div>
</template>

<script>
export default {
  
}
</script>
<style>
.def-layout{
  display: grid;
  /* height: 100vh; */
  width: 100%;
  grid-template-areas: 'top-bar top-bar'
  'side-bar main';
  grid-template-rows: 52px 1fr;
  grid-template-columns: 400px 1fr; 
}

.top-bar{
  grid-area: top-bar;
}

.side-bar{
  grid-area: side-bar;
}

.main{
  grid-area: main;
}
</style>
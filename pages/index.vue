<template>
  <main class="main">
    <div class="chat-area"></div>

    <UserPanel></UserPanel>
  </main>
  
</template>

<script>
export default {}
</script>
<style>
.main{
 display: flex;
 height: calc(100vh - 52px);
}

.chat-area{
  flex: 1;
}

</style>
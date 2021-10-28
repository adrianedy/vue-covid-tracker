<template>
  <Header />
  <div class="container">
    <router-view/>
  </div>
</template>

<script>
import Header from '@/components/Header'
export default {
  components: {
    Header,
  }
}
</script>

<style>
  @import "https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta2/css/all.min.css";
</style>
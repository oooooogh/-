<template>
  <div class="home">
    <Header />
    <div class="main-content">
      <PostList />
      <Sidebar />
    </div>
    <Footer />
  </div>
</template>

<script>
  import Header from '@/components/Header.vue'
  import Sidebar from '@/components/Sidebar.vue'
  import PostList from '@/components/PostList.vue'
  import Footer from '@/components/Footer.vue'

  export default {
    components: {
      Header,
      PostList,
      Sidebar,
      Footer
    }
  }
</script>

<style scoped>
  .home {
    font-family: Arial, sans-serif;
  }

  .main-content {
    display: flex;
  }
</style>

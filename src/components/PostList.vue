<template>
  <div class="post-list">
    <p v-if="posts.length == 0">{{ mes }}</p>
    <PostItem
        v-for="(post, index) in posts"
        :key="index"
        :likes="post.likes"
        :title="post.title"
        :content="post.content"
        :username="post.username"
        :time="post.time"
    />
    <!-- 提示加载 -->
    <div v-if="isLoading" class="loading">{{ mes }}</div>
  </div>
</template>
  
  <script>
  import PostItem from "./PostItem.vue";
  import axios from "axios";
  
  export default {
    name: "PostList",
    components: {
      PostItem,
    },
    data() {
      return {
        mes: "loading...",
        posts: [],   // 存放帖子的数组
        page: 0,     // 当前页码，初始值为0
        size: 5,  // 每页请求的帖子数
        isLoading: false, // 是否正在加载数据
      };
    },
    mounted() {
      // 在组件加载时请求数据
      this.fetchPosts();
      // 添加滚动监听
      window.addEventListener("scroll", this.handleScroll);
    },
    beforeDestroy() {
      // 移除滚动监听
      window.removeEventListener("scroll", this.handleScroll);
    },
    methods: {
      // 请求数据的方法
      fetchPosts() {
        if (this.isLoading) return; // 防止重复加载
        this.isLoading = true;      // 标记加载中
        // 发起 GET 请求，获取帖子列表数据
        axios.get(`http://127.0.0.1:8080/api/v1/posts?page=${this.page}&content=${this.size}`)
          .then(response => {
            if (response.data.code === 1000) {
              // 请求成功，更新 posts 数据
              this.posts = [...this.posts, ...response.data];
              // 更新 page 变量，每次请求后加一
              this.page += 1;
              this.isLoading = false; // 加载完成
            }
            else {
              console.error('请求失败:', error);
              this.isLoading = false; // 加载完成
            }
          })
          .catch(error => {
            console.error('请求失败:', error);
            this.isLoading = false; // 加载完成
          });
      },
      // 处理滚动事件
      handleScroll() {
        const scrollTop = document.documentElement.scrollTop; // 已滚动高度
        const clientHeight = document.documentElement.clientHeight; // 视口高度
        const scrollHeight = document.documentElement.scrollHeight; // 页面总高度

        // 判断是否到达底部
        if (scrollTop + clientHeight >= scrollHeight - 10) {
          this.fetchPosts(); // 加载更多数据
        }
      },
    },
  };
</script>
  
  <style scoped>
  .post-list {
    display: flex;
    flex-direction: column;
    gap: 20px; /* 每个帖子的间距 */
    width: 100%; /* 列表宽度最大化 */
    max-width: 1500px; /* 最大宽度 */
    margin: 0 auto;
    padding: 20px;
    height: 800px; /* 增加高度，使列表更长 */
    overflow-y: auto; /* 如果内容过长，允许滚动 */
  }
  </style>
  
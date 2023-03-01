<template>
  <div class="column-article-list">
    <transition-group name="animate__animated animate__bounce"
                      tag="div">
      <div v-for="item,index of result"
           :key="item.id"
           class="animate__fadeInLeft"
           :style="{'animation-delay':(500 + index * 200)+'ms'}">
        <div class="column-article-card">
          <el-card shadow="hover">
            <router-link :to="'/column/columnArticlePage/' +this.result[index].id">
              <h2 class="title">{{item.title}}</h2>
            </router-link>
            <div class="user-info">
              <img :src="item.creator.avatar"
                   class="user-avatar" />
              <p class="user-name">{{item.creator.username}}</p>
              <p class="user-intro">, {{item.creator.introduction}}</p>
            </div>
            <div class=page>
              <div cover-image>
                <img :src="item.cover_img"
                     class="cover-image" />
              </div>

              <div class="column-article-list-body">
                {{item.content}}
              </div>
            </div>

            <div class="div-tool-bar">
              <article-tool-bar class="tool-bar"
                                :article="item"></article-tool-bar>
            </div>
          </el-card>
        </div>
      </div>
    </transition-group>
  </div>

</template>
  
  <script>
import ArticleToolBar from './ArticleToolBar.vue'
export default {
  name: 'ColumnArticleList',
  props: {
    columnId: {
      type: Number,
      default: 0,
    },
  },
  components: {
    ArticleToolBar,
  },
  data() {
    return {
      result: [],
    }
  },
  methods: {
    getArticleList() {
      let url = '/api/column/getColumnArticles'
      let params = new URLSearchParams()
      params.append('columnId', this.$route.params.id)
      params.append('currentPage', 1)
      params.append('pageSize', 100)
      this.$http.get(url, { params: params }).then((res) => {
        console.log(res.data.data)
        this.result = res.data.data
      })
    },
  },
  mounted() {
    this.getArticleList()
  },
}
</script>
  
  <style scoped>
.title {
  font-size: 22px;
  font-weight: bold;
  margin: 0;
  padding: 0;
  color: #000000;
}

.title:hover {
  color: #175199;
}

.column-article-list {
  flex: 1 1 auto;
}

:deep(.el-card__body) {
  background-color: #fefbf4;
}

.title {
  margin-top: 0px;
  margin-bottom: 10px;
  font-family: sans-serif;
}
.user-info {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}
.user-avatar {
  width: 25px;
}
.user-name {
  /* 加粗 */
  font-weight: bold;
  font-family: sans-serif;
  margin-left: 15px;
  font-size: 13px;
}
.user-intro {
  /* font-size: 12px; */
  color: #999;
  font-family: sans-serif;
  margin-left: 5px;
  font-size: 13px;
}

.page {
  display: flex;
  max-height: 200px;
}

.column-article-card:deep(.el-card__body) {
  display: flex;
  flex-direction: column;
}
.column-article-card :deep(.el-col-1) {
  max-width: 10%;
}

.cover-image {
  max-width: 150px;
  max-height: 100px;
}

.column-article-card :deep(.el-card_header) {
  background-color: #fefbf4;
  height: 65px;
  padding: 0;
  padding-left: 10px;
}
.column-article-card :deep(.column-article-list-body) {
  overflow: hidden;
  text-overflow: ellipsis;
  color: black;
  max-height: 100px;
  font-family: sans-serif;
  margin-left: 15px;
}

.acticle-tool-bar {
  margin-top: 30px;
  margin-left: 0;
  padding-left: 0;
  justify-content: left;
}
</style>
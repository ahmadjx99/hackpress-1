<template>
  <div>
    <div>
      <div >
        <form method="post" @submit.prevent="addArticle()">
          <input type="text" name="article" placeholder="input article " v-model="post.title">
          <input type="text" name="article" placeholder="input article " v-model="post.content">
          <input type="text" name="article" placeholder="input article " v-model="post.category">
          <button name="button">add article</button>
        </form>
      </div>
      <div>
        <tbody>
          <tr v-for="article in articles">
            <td>{{ article.title }}</td>
            <td>{{ article.content }}</td>
            <td>{{ article.category }}</td>
            <td>{{ article.author }}</td>
            <td><button type="submit" @click="deleteArticle(article._id)">hapus</button></td>
          </tr>
        </tbody>
      </div>
      <div>
        <button type="button" class="" @click="logout()">Logout</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      articles: [],
      post: {
        title: '',
        content: '',
        category: ''
      }
    }
  },
  methods: {
    getArticle () {
      this.$http.get('/articles', {
      })
      .then(result => {
        console.log('result', result.data)
        this.articles = result.data
      })
      .catch(err => {
        console.error(err)
      })
    },
    addArticle () {
      this.$http.post('/articles', {
        title: this.post.title,
        content: this.post.content,
        category: this.post.category
      }, {
        headers: {
          token: localStorage.getItem('token')
        }
      })
      .then(result => {
        console.log('result add', result)
        this.articles.push(result.data)
      })
      .catch(err => {
        console.error(err)
      })
    },
    deleteArticle (data, index) {
      this.$http.delete(`/article/${data}`, {
        headers: {
          token: localStorage.getItem('token')
        }
      })
      .then(response => {
        this.articles.splice(index, 1)
      })
    },
    logout () {
      localStorage.clear()
    }
  },
  created () {
    this.getArticle()
  }
}
</script>
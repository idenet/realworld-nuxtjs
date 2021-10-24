<template>
  <div class="editor-page">
    <div class="container page">
      <div class="row">
        <div class="col-md-10 offset-md-1 col-xs-12">
          <form @submit.prevent>
            <fieldset>
              <fieldset class="form-group">
                <input
                  type="text"
                  class="form-control form-control-lg"
                  placeholder="Article Title"
                  v-model="article.title"
                />
              </fieldset>
              <fieldset class="form-group">
                <input
                  type="text"
                  class="form-control"
                  placeholder="What's this article about?"
                  v-model="article.description"
                />
              </fieldset>
              <fieldset class="form-group">
                <textarea
                  class="form-control"
                  rows="8"
                  placeholder="Write your article (in markdown)"
                  v-model="article.body"
                ></textarea>
              </fieldset>
              <fieldset class="form-group">
                <input
                  type="text"
                  class="form-control"
                  placeholder="Enter tags"
                  v-model="tag"
                  @keyup.enter="addTagList"
                />
                <div
                  class="tag-list"
                  v-for="(tag, index) in tagList"
                  :key="tag"
                >
                  <span class="tag-default tag-pill ng-binding ng-scope">
                    <i class="ion-close-round" @click="removeTag(index)"></i>
                    {{tag}}
                  </span>
                </div>
              </fieldset>
              <button
                class="btn btn-lg pull-xs-right btn-primary"
                type="button"
                @click.prevent="create"
              >Publish Article</button>
            </fieldset>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { createArticle } from '@/api/article'
export default {
  // 在路由匹配组件渲染之前会先执行中间件处理
  middleware: 'authenticated',
  name: 'EditorIndex',
  data() {
    return {
      article: {},
      tag: '',
      tagList: []
    }
  },
  methods: {
    async create() {
      const { data } = await createArticle({
        article: { ...this.article, tagList: this.tagList }
      })
      this.$router.push({ name: 'article', params: { slug: data.article.slug } })
    },
    addTagList() {
      let index = this.tagList.findIndex(v => v === this.tab)
      if (index == -1) {
        this.tagList.push(this.tag)
      }
    },
    removeTag(index) {
      this.tagList.splice(index, 1)
    }
  },
}
</script>

<style>
</style>

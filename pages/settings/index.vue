<template>
  <div class="settings-page">
    <div class="container page">
      <div class="row">
        <div class="col-md-6 offset-md-3 col-xs-12">
          <h1 class="text-xs-center">Your Settings</h1>

          <form @submit.prevent>
            <fieldset>
              <fieldset class="form-group">
                <input
                  class="form-control"
                  type="text"
                  placeholder="URL of profile picture"
                  v-model="pageUser.image"
                />
              </fieldset>
              <fieldset class="form-group">
                <input
                  class="form-control form-control-lg"
                  type="text"
                  placeholder="Your Name"
                  v-model="pageUser.username"
                />
              </fieldset>
              <fieldset class="form-group">
                <textarea
                  class="form-control form-control-lg"
                  rows="8"
                  placeholder="Short bio about you"
                  v-model="pageUser.bio"
                ></textarea>
              </fieldset>
              <fieldset class="form-group">
                <input
                  class="form-control form-control-lg"
                  type="text"
                  placeholder="Email"
                  v-model="pageUser.email"
                />
              </fieldset>
              <!-- <fieldset class="form-group">
                <input
                  class="form-control form-control-lg"
                  type="password"
                  placeholder="Password"
                  v-model="pageUser.password"
                />
              </fieldset>-->
              <button
                @click="update"
                :disabled="disabled"
                class="btn btn-lg btn-primary pull-xs-right"
              >Update Settings</button>
            </fieldset>
            <br />
            <button
              class="btn btn-outline-danger"
              @click="logOut"
            >Or click here to logout.</button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState } from 'vuex'
import { updateUser } from '@/api/user'
const Cookie = process.client ? require('js-cookie') : undefined
export default {
  middleware: ['authenticated'],
  name: 'SettingsIndex',
  data() {
    return {
      pageUser: {},
      disabled: false
    }
  },
  computed: {
    ...mapState(['user'])
  },
  created() {
    this.pageUser = Object.assign(this.pageUser, this.user)
  },
  methods: {
    async update() {
      this.disabled = true
      const { data } = await updateUser({
        user: this.pageUser
      })
      this._dataPersistence(data)
      this.disabled = false
      this.$router.push(`/profile/${data.username}`)
    },
    _dataPersistence(data) {
      // TODO: 保存用户的登录状态
      this.$store.commit('setUser', data.user)

      // 为了防止刷新页面数据丢失，我们需要把数据持久化
      Cookie.set('user', JSON.stringify(data.user))
    },
    logOut() {
      // 清空登录状态

      Cookie.remove('user')
      this.$store.commit('setUser', null)
      this.$router.push(`/`)
    }
  }
}
</script>

<style>
</style>

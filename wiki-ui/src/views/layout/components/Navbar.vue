<template>
  <div class="navbar">
    <h1 class="show-navbar-title" style="margin:0px;line-height:60px;float:left;font-size:20px;padding-right:40px;">
      <a href="/" class="router-link-active">
        {{ title }}
      </a>
    </h1>
    <div class="menu" style="float:left">
      <el-menu
        :default-active="activeIndex"
        :router="true"
        class="el-menu-demo"
        mode="horizontal"
        background-color="#545c64"
        text-color="#fff"
        active-text-color="#ffd04b"
        @select="handleSelect">
        <el-menu-item index="/index">
          首页
        </el-menu-item>
        <el-menu-item index="/article" >
          文章
        </el-menu-item>
        <el-menu-item index="/doc" >
          文档
        </el-menu-item>

        <!-- <el-menu-item v-if="token != '' && token != null" index="/admin/dashboard" >
          系统管理
        </el-menu-item> -->
      </el-menu>
    </div>

    <div style="float:right;" >
      <el-menu
        v-if="isLogin"
        :default-active="activeIndex"
        :router="true"
        class="el-menu-demo"
        mode="horizontal"
        background-color="#545c64"
        text-color="#fff"
        active-text-color="#ffd04b"
        @select="handleSelect">
        <el-submenu index="/">
          <template slot="title">
            <avatar :username="username" :src="avatar" :size="40" style="float:left;margin-top:10px;" />
          </template>
          <el-menu-item index="/admin/user" >个人中心</el-menu-item>
          <el-menu-item index="/admin/article" >文章管理</el-menu-item>
          <el-menu-item index="/admin/doc" >文档管理</el-menu-item>
          <el-menu-item v-if="isAdmin" index="/admin/system" >系统管理</el-menu-item>
          <el-menu-item index="/login" @click="logout">退出登陆</el-menu-item>
        </el-submenu>
      </el-menu>
      <div v-else>
        <el-button type="text" @click="login" >登陆</el-button>
        <el-divider direction="vertical"/>
        <el-button type="text" @click="login" >注册</el-button>
      </div>
    </div>
  </div>
</template>
<script>
import Avatar from 'vue-avatar'
import { mapGetters } from 'vuex'
import { getToken } from '@/utils/auth'
import { isAdmin, isLogin } from '@/utils/authUtils'
export default {
  components: {
    Avatar
  },
  data() {
    return {
      isLogin: isLogin(),
      isAdmin: isAdmin(),
      token: getToken(),
      activeIndex: this.$router.currentRoute.path
    }
  },
  computed: {
    ...mapGetters([
      'userId',
      'username',
      'avatar',
      'title'
    ])
  },
  methods: {
    handleSelect(key, keyPath) {
      this.activeIndex = key
    },
    login() {
      this.$router.push({ path: '/login' })
    },
    logout() {
      this.$store.dispatch('LogOut').then(() => {
        this.$router.push({ path: this.redirect || '/login' })
      })
    }
  }
}
</script>

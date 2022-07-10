<template>
  <header class="header">
    <h1 class="header__title">Atte</h1>
    <nav class="header__nav">
      <ul class="nav__menu" v-if="!$auth.loggedIn">
        <li class="nav__menu-list">
          <NuxtLink to="/register">Register</NuxtLink>
        </li>
        <li class="nav__menu-list">
          <NuxtLink to="/login">Login</NuxtLink>
        </li>
      </ul>
      <ul class="nav__menu" v-else>
        <li class="nav__menu-list">
          <a>ホーム</a>
        </li>
        <li class="nav__menu-list">
          <a>日付一覧</a>
        </li>
        <li class="nav__menu-list">
          <a @click="logout">Logout</a>
        </li>
      </ul>
    </nav>
  </header>
</template>

<script>
export default {
  methods: {
    async logout() {
      try {
        await this.$auth.logout();
        this.$router.push("/login");
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style>
.header {
  display: flex;
  justify-content: space-between;
  height: 100px;
  padding: 0 50px;
  margin-bottom: 30px;
  background-color: #ffffff;
  align-items: center;
}
.header__title {
  margin-right: auto;
  font-size: 32px;
}
.nav__menu-list {
  list-style: none;
  display: inline-block;
  padding: 10px;
}
.nav__menu-list a {
  color: black;
  text-decoration: none;
  cursor: pointer;
}
</style>

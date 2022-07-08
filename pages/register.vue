<template>
  <div class="auth__container">
    <form @submit.prevent="register">
      <validation-observer ref="obs" v-slot="ObserverProps">
        <validation-provider v-slot="{ errors }" rules="required|min:4|max:255">
          <input v-model="name" type="text" name="名前" placeholder="名前" />
          <div class="error">{{ errors[0] }}</div>
        </validation-provider>
        <validation-provider v-slot="{ errors }" rules="required|email">
          <input v-model="email" type="email" name="メールアドレス" placeholder="メールアドレス"/>
          <div class="error">{{ errors[0] }}</div>
        </validation-provider>
        <validation-provider v-slot="{ errors }" vid="passwordConfirm" rules="required|min:8|alpha_dash">
          <input v-model="password" type="password" name="パスワード" placeholder="パスワード"/>
          <div class="error">{{ errors[0] }}</div>
        </validation-provider>
        <validation-provider v-slot="{ errors }" rules="confirmed:passwordConfirm">
          <input v-model="passwordConfirm" type="password" name="確認用パスワード" placeholder="確認用パスワード"/>
          <div class="error">{{ errors[0] }}</div>
        </validation-provider>
        <button type="submit" :disabled="ObserverProps.invalid || !ObserverProps.validated">会員登録</button>
      </validation-observer>
    </form>
  </div>
</template>

<script>
export default {
  auth: false,
  data() {
    return {
      name: null,
      email: null,
      password: null,
      passwordConfirm: null,
    };
  },
  methods: {
    async register() {
      try {
        await this.$axios.post("http://localhost:8000/api/auth/register", {
          name: this.name,
          email: this.email,
          password: this.password,
        });
        this.$router.push("/login");
      } catch {
        alert("メールアドレスがすでに登録されています");
      }
    },
  },
};
</script>

<template>
   <validation-observer ref="obs" v-slot="ObserverProps">
    <form @submit.prevent="login" class="inner-elements">
      <validation-provider v-slot="{ errors }" rules="required|email">
         <div class="error">{{ errors[0] }}</div>
         <div class="element">
           <input
             type="email"
             v-model="email"
             placeholder="email"
             id="email"
           />
         </div>
      </validation-provider>
      <validation-provider
        v-slot="{ errors }"
        vid="passwordConfirm"
        rules="required"
      >
      <div class="error">{{ errors[0] }}</div>
      <div class="element">
        <input
          type="password"
          v-model="password"
          placeholder="password"
          id="password"
        />
      </div>
      </validation-provider>
      <button
        type="submit"
        class="login-button"
        :disabled="ObserverProps.invalid || !ObserverProps.validated"
      >
        ログイン
      </button>
    </form>
  </validation-observer>
</template>

<script>
export default {
  data() {
    return {
      email: null,
      password: null,
    };
  },
  methods: {
    async login() {
      try {
        const res = await this.$auth.loginWith("laravelJWT", {
          data: {
            email: this.email,
            password: this.password,
          }
        });
        this.$router.push("/");
      } catch(error)  {
        console.log(error);
        alert("メールアドレスまたはパスワードが間違っております");
        this.loginEmail = "";
        this.loginPassword = "";
      }
    },
  },
};
</script>

<template>
  <div class="container">
    <p v-if="$auth.loggedIn">Name:{{ $auth.user }}</p>

    <button @click="attendanceStart">勤務開始</button>
    <button>勤務終了</button>
    <button>休憩開始</button>
    <button>休憩終了</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      attendanceLists:[],
    }
  },
  methods: {
    async attendanceGet() {
      const resData = await this.$axios.get("http://localhost:8000/api/auth/attendance");
      this.attendanceLists = resData.data.attendances;
      console.log(resData.data.attendances);
    },
    async attendanceStart() {
      const sendData = {
         'user_id': this.$auth.user.id,
      };
      const resData = await this.$axios.post("http://localhost:8000/api/auth/attendance",sendData);
      console.log(resData);
      this.attendanceGet();
    },
  },
  created() {
    this.attendanceGet();
  }
}
</script>

<style>

</style>

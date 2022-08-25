<template>
  <div class="container">
    <p v-if="$auth.loggedIn">Name:{{ $auth.user }}</p>

    <button @click="attendanceStart">勤務開始</button>
    <p>勤務開始</p>
    <button @click="attendanceEnd">勤務終了</button>
    <button v-if="display = !display" @click="restStart">休憩開始</button>
    <p v-else>B</p>
    <button v-if="display" @click="restEnd">休憩終了</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      attendanceLists: [],
      display: true,
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
    async attendanceEnd() {
      const user_id = this.$auth.user.id;
      const today = this.$dayjs().format('YYYY-MM-DD');
      const attendance = this.attendanceLists.find(function (element) {
        if(element.user_id == user_id && element.date == today) {
          return element;
        }
      });
      const resData = await this.$axios.put("http://localhost:8000/api/auth/attendance/"+ attendance.id);
      console.log(resData);
      this.attendanceGet();
    },
    async restStart() {
      const sendData = {
         'user_id': this.$auth.user.id,
      };
      const resData = await this.$axios.post("http://localhost:8000/api/auth/rest",sendData);
      console.log(resData);
      this.attendanceGet();
    },
    async restEnd() {
      const user_id = this.$auth.user.id;
      const today = this.$dayjs().format('YYYY-MM-DD');
      const attendance = this.attendanceLists.find(function (element) {
        if(element.user_id == user_id && element.date == today) {
          return element;
        }
      });
      const rest = attendance.rests.find(function (element) {
        if(element.end_time == null) {
          return element;
        }
      });
      const resData = await this.$axios.put("http://localhost:8000/api/auth/rest/"+ rest.id);
      console.log(resData);
      this.attendanceGet();
    },
  },
  computed: {
    attendanceCheck: function() {
      var user_id = this.$auth.user.id;
      var today = this.$dayjs().format('YYYY-MM-DD');
      this.attendanceLists.find(function (element) {
        if(element.user_id == user_id && element.date == today) {
          consoloe.log("a");
          console.log(element);
          console.log(element.end_time);
          return element.end_time == null;
        }else {
          consoloe.log("b");
        }
      });
    },
  },
  created() {
    this.attendanceGet();
  }
}
</script>

<style>

</style>

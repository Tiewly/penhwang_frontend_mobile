<template>
  <v-card class="mx-auto" max-width="400">
    <v-img
      class="black--text align-end"
      height="200px"
      src="https://www.img.in.th/images/5dfbbff4a010135e3f6ff5a84fc99499.png?fbclid=IwAR0hDONZV89-e2Ij3hRHc5Gy-m7wgz0R2yi6k2emkwn0k9dYKTvZ9_oOADs"
    >
      <v-card-title
        ><h2 style="background-color: white; padding: 5px">
          แบบฟอร์มการลางาน
        </h2></v-card-title
      >
    </v-img>
    <div style="background-color: #f5f5f5; height: 100%">
      <v-form v-model="valid" lazy-validation style="padding: 5% 5% 5% 5%">
        <v-select
          v-model="leaveType"
          :items="items"
          :rules="[(v) => !!v || 'Leave type is required']"
          required
          label="ประเภทการลา"
          outlined
          solo
        ></v-select>

        <h3 style="color: #43a047">Start Date Time</h3>
        <br />
        <DateTimeInput
          @onDateChange="(d) => (date1 = d)"
          @onTimeChange="(t) => (time1 = t)"
        />
        <h3 style="color: #ab47bc">End Date Time</h3>
        <br />
        <DateTimeInput
          @onDateChange="(d) => (date2 = d)"
          @onTimeChange="(t) => (time2 = t)"
        />
        <h3>เหตุผล</h3>
        <v-textarea
          v-model="reason"
          :rules="reasonRules"
          required
          auto-grow
          dense
          outlined
          rows="2"
          row-height="20"
          hide-details
          background-color="#FFFFFF"
        ></v-textarea>
        <br />
        <v-spacer />
        <v-btn :disabled="!valid" block color="primary" large @click="sendForm">
          <h3>ส่งแบบฟอร์ม</h3>
        </v-btn>
        <br />
      </v-form>
    </div>
  </v-card>
</template>

<script>
import api from '~/api/index'
export default {
  data: () => ({
    date1: null,
    date2: null,
    time1: null,
    time2: null,
    items: ['ลาป่วย', 'ลากิจ', 'ลาคลอด', 'ลาทำหมัน'],
    valid: false,
    leaveType: 'null',
    leaveId: '',
    reason: null,
    loading: true,
    reasonRules: [(v) => !!v || 'Reason is required'],
  }),
  computed: {
    dateTime1() {
      return new Date(`${this.date1}T${this.time1}+07:00`)
    },
    dateTime2() {
      return new Date(`${this.date2}T${this.time2}+07:00`)
    },
  },
  methods: {
    sendForm() {
      var leaveData = {
        employeeId: this.$route.params.id,
        leaveId: this.getLeaveId(this.leaveType),
        reason: this.reason,
        start: this.dateTime1,
        end: this.dateTime2,
      }
      console.log(leaveData);
    },
    getLeaveId(leaveName) {
      //TODO get leave id from leave name
      return '123'
    },
  },
  mounted() {
    this.$nextTick(() => {
      this.$nuxt.$loading.start();
      this.$axios
        .$post(api.getProfile, {employeeId: this.$route.params.id})
        .then((res) => {
          if(res.isSuccess){
            this.employee = res.data.employee ;
            this.req = res.data.requests ;
            this.day = res.data.leaveRight ;
            this.loading = false;
          }else{
            console.log(res);
          }
        })
        .catch((err) => console.error(err))
      this.$nuxt.$loading.finish();
    })
  },
}
</script>


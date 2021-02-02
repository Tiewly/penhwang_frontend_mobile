<template>
  <div style="background-color: #f5f5f5; height: 100%">
    <v-form v-model="valid" lazy-validation style="padding: 5% 5% 5% 5%">
      <h2>แบบฟอร์มการลางาน</h2>
      <br />
      <v-select
        v-model="leaveType"
        :items="items"
        :rules="[(v) => !!v || 'Leave type is required']"
        required
        label="ประเภทการลา"
        dense
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
      <v-btn :disabled="!valid" block color="primary" large>
        <h3>ส่งแบบฟอร์ม</h3>
      </v-btn>
      <br />
      <!-- <h3>test</h3>
    <p>date1: {{date1}}</p>
    <p>date2: {{date2}}</p>
    <p>time1: {{time1}}</p>
    <p>time2: {{time2}}</p> -->
    </v-form>
  </div>
</template>

<script>
export default {
  data: (vm) => ({
    date1: null,
    date2: null,
    time1: null,
    time2: null,
    items: ['ลาป่วย', 'ลากิจ', 'ลาคลอด', 'ลาทำหมัน'],
    valid: false,
    leaveType: null,
    reason: null,
    reasonRules: [(v) => !!v || 'Reason is required'],
  }),
}
</script>


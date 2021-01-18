<template>
  <div>
    <h2>แบบฟอร์มการลางาน</h2>
    <v-select :items="items" label="ประเภทการลา" dense outlined></v-select>
    <DateTimeInput
      header="Start Date Time"
      @onDateChange="(d) => (date1 = d)"
      @onTimeChange="(t) => (time1 = t)"
    />
    <DateTimeInput
      header="End Date Time"
      @onDateChange="(d) => (date2 = d)"
      @onTimeChange="(t) => (time2 = t)"
    />
    <h3>เหตุผล</h3>
    <v-textarea auto-grow outlined rows="2" row-height="20"></v-textarea>
    <v-spacer />
    <v-btn block color="primary" nuxt to="/inspire" large> ส่งแบบฟอร์ม </v-btn>
    <!-- <h3>test</h3>
    <p>date1: {{date1}}</p>
    <p>date2: {{date2}}</p>
    <p>time1: {{time1}}</p>
    <p>time2: {{time2}}</p> -->
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
  }),
}
</script>


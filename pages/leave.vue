<template>
  <div>
    <h1>แบบฟอร์มการลางาน</h1>
    <v-select :items="items" label="ประเภทการลา" dense outlined></v-select>
    <h3>Start Date Time</h3>
    <br />
    <v-menu
      ref="DateStart"
      v-model="DateStart"
      :close-on-content-click="false"
      transition="scale-transition"
      offset-y
      max-width="290px"
      min-width="290px"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-text-field
          v-model="dateFormatted"
          label="วัน/เดือน/ปี"
          persistent-hint
          prepend-icon="mdi-calendar"
          v-bind="attrs"
          @blur="date = parseDate(dateFormatted)"
          v-on="on"
          outlined
        ></v-text-field>
      </template>
      <v-date-picker
        v-model="date"
        no-title
        @input="DateStart = false"
      ></v-date-picker>
    </v-menu>
    <v-menu
      ref="TimeStart"
      v-model="TimeStart"
      :close-on-content-click="false"
      :nudge-right="40"
      :return-value.sync="time"
      transition="scale-transition"
      offset-y
      max-width="290px"
      min-width="290px"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-text-field
          v-model="time"
          label="Time"
          prepend-icon="mdi-clock-time-four-outline"
          readonly
          v-bind="attrs"
          v-on="on"
          outlined
        ></v-text-field>
      </template>
      <v-time-picker
        v-if="TimeStart"
        v-model="time"
        full-width
        @click:minute="$refs.TimeStart.save(time)"
      ></v-time-picker>
    </v-menu>
    <h3>End Date Time</h3>
    <br />
    <v-menu
      ref="DateEnd"
      v-model="DateEnd"
      :close-on-content-click="false"
      transition="scale-transition"
      offset-y
      max-width="290px"
      min-width="290px"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-text-field
          v-model="date2Formatted"
          label="วัน/เดือน/ปี"
          persistent-hint
          prepend-icon="mdi-calendar"
          v-bind="attrs"
          @blur="date2 = parseDate(date2Formatted)"
          v-on="on"
          outlined
        ></v-text-field>
      </template>
      <v-date-picker
        v-model="date2"
        no-title
        @input="DateEnd = false"
      ></v-date-picker>
    </v-menu>
    <v-menu
      ref="TimeEnd"
      v-model="TimeEnd"
      :close-on-content-click="false"
      :nudge-right="40"
      :return-value.sync="time"
      transition="scale-transition"
      offset-y
      max-width="290px"
      min-width="290px"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-text-field
          v-model="time2"
          label="Time"
          prepend-icon="mdi-clock-time-four-outline"
          readonly
          v-bind="attrs"
          v-on="on"
          outlined
        ></v-text-field>
      </template>
      <v-time-picker
        v-if="TimeEnd"
        v-model="time2"
        full-width
        @click:minute="$refs.TimeEnd.save(time2)"
      ></v-time-picker>
    </v-menu>
    <h3>เหตุผล</h3>
    <v-textarea auto-grow outlined rows="2" row-height="20"></v-textarea>
    <v-spacer />
    <v-btn block color="primary" nuxt to="/inspire" large> ส่งแบบฟอร์ม </v-btn>
  </div>
</template>

<script>
export default {
  data: (vm) => ({
    date: new Date().toISOString().substr(0, 10),
    date2: new Date().toISOString().substr(0, 10),
    dateFormatted: vm.formatDate(new Date().toISOString().substr(0, 10)),
    date2Formatted: vm.formatDate(new Date().toISOString().substr(0, 10)),
    DateStart: false,
    TimeStart: false,
    DateEnd: false,
    TimeEnd: false,
    items: ['ลาป่วย', 'ลากิจ', 'ลาคลอด', 'ลาทำหมัน'],
  }),

  computed: {
    computedDateFormatted() {
      return this.formatDate(this.date)
    },
  },

  watch: {
    date(val) {
      this.dateFormatted = this.formatDate(this.date)
    },
    date2(val) {
      this.date2Formatted = this.formatDate(this.date2)
    },
  },

  methods: {
    formatDate(date) {
      if (!date) return null

      const [year, month, day] = date.split('-')
      return `${day}/${month}/${year}`
    },
    parseDate(date) {
      if (!date) return null

      const [month, day, year] = date.split('/')
      return `${year}-${month.padStart(2, '0')}-${day.padStart(2, '0')}`
    },
  },
}
</script>


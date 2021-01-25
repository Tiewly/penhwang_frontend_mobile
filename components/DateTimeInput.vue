<template>
  <div>
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
          v-on="on"
          outlined
          dense
        ></v-text-field>
      </template>
      <v-date-picker
        @change="$emit('onDateChange', date)"
        v-model="date"
        no-title
        @input="DateStart = false"
      ></v-date-picker>
    </v-menu>
    <v-text-field
      v-model="time"
      @change="$emit('onTimeChange', time)"
      label="เวลา"
      type="time"
      prepend-icon="mdi-clock"
      outlined
      dense
    ></v-text-field>
  </div>
</template>
<script>
export default {
  props: ['header'],
  data: (vm) => ({
    date: new Date().toISOString().substr(0, 10),
    dateFormatted: vm.formatDate(new Date().toISOString().substr(0, 10)),
    DateStart: false,
    TimeStart: false,
    time: null,
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
    timeNow() {
      var d = new Date(),
        h = (d.getHours() < 10 ? '0' : '') + d.getHours(),
        m = (d.getMinutes() < 10 ? '0' : '') + d.getMinutes()
      return h + ':' + m
    },
  },
  mounted() {
    this.time = this.timeNow()
  },
}
</script>

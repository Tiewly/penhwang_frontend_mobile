<template>
  <v-card class="mx-auto" max-width="400">
    <v-img
      class="black--text align-end"
      height="200px"
      src="https://uppic.cc/d/YOGrE0dYW8UiINrkFA1JT?fbclid=IwAR3OC1dmQ5hHtTwC6mkhWoBvsPI8DbeZtiYWyABCV8kyxBjnkZ2BRmuumqE"
    >
      <v-card-title
        ><h2 style="background-color: white; padding: 5px">
          ประวัติการทำงาน
        </h2></v-card-title
      >
    </v-img>
    <v-text-field
      v-model="search"
      label="วันที่"
      append-icon="mdi-magnify"
      outlined
      hide-details
      dense
      style="padding: 5% 5% 3% 5%"
    />
    <v-data-table
      :headers="headers"
      :items="workHourFormated"
      :items-per-page="5"
      :search="search"
      mobile-breakpoint
    >
      <template v-slot:item.in="{ item }">
        <v-chip :color="getColor(item.inErr)" dark>
          {{ item.inDate }}
        </v-chip>
      </template>
      <template v-slot:item.out="{ item }">
        <v-chip :color="getColor(item.outErr)" dark>
          {{ item.outDate }}
        </v-chip>
      </template>
    </v-data-table>
  </v-card>
</template>

<script>
export default {
  data: () => ({
    search: '',
    headers: [
      { text: 'วัน/เดือน/ปี', value: 'date', align: 'center' },
      { text: 'เวลาเข้า', value: 'in', align: 'center' },
      { text: 'เวลาออก', value: 'out', align: 'center' },
      { text: 'รวม (hr:min)', value: 'sum', align: 'center' },
    ],
    workHour: [
      {
        date: '01/10/2020',
        amount: 8,
        attendance: [
          {
            time: 'Sat Jan 23 2021 08:33:35 GMT+0700 (Indochina Time)',
            errorTime: 18,
            type: 1,
          },
          {
            time: 'Sat Jan 23 2021 17:33:35 GMT+0700 (Indochina Time)',
            errorTime: -1,
            type: 2,
          },
        ],
      },
    ],
  }),
  computed: {
    workHourFormated() {
      const whs = this.workHour
      var r = []
      whs.forEach((wh) => {
        var i = -1
        wh.attendance.forEach((att) => {
          if (att.type == 1) {
            r.push({
              date: wh.date,
              in: new Date(att.time),
              inDate: this.dateFormated(new Date(att.time)),
              inErr: att.errorTime,
              inAddr: att.location,
              out: '',
              outDate: '',
              outErr: 1,
              outAddr: '',
              sum: '',
            })
            i++
          } else {
            r[i].out = new Date(att.time)
            ;(r[i].outDate = this.dateFormated(new Date(att.time))),
              (r[i].outErr = att.errorTime)
            r[i].outAddr = att.location
            var sum = (r[i].out - r[i].in) / 1000 / 60
            var h = this.shiftZero(sum / 60)
            var m = this.shiftZero(sum % 60)
            r[i].sum = h + ':' + m
          }
        })
      })
      return r
    },
  },
  methods: {
    getColor(i) {
      if (i > 0) return 'success'
      else return 'warning'
    },
    shiftZero(m) {
      if (m >= 10) return `${m}`
      else if (m > 0) return `0${m}`
      else return '00'
    },
    dateFormated(d) {
      var m = this.shiftZero(d.getMinutes())
      var h = this.shiftZero(d.getHours())
      return `${h}:${m}`
    },
  },
}
</script>
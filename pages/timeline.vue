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
    <v-row>
      <v-col style="margin-left: 3%; margin-right: 3%">
        <br />
        <v-row>
          <v-col style="max-width: 180px"
            ><v-text-field
              v-model="start"
              type="date"
              label="จากวันที่"
              dense
              outlined
              hide-details
            ></v-text-field
          ></v-col>
          <v-col style="max-width: 180px"
            ><v-text-field
              v-model="end"
              type="date"
              label="ถึงวันที่"
              dense
              outlined
              hide-details
            ></v-text-field
          ></v-col>
          <v-btn
            block
            color="info"
            @click="$emit('getWorkHour', { start, end })"
            >ค้นหา</v-btn
          >
        </v-row>
        <br />
        <v-data-table
          :headers="headers"
          :items="workHourFormated"
          :items-per-page="5"
          mobile-breakpoint
        >
          <!-- <template v-slot:item.in="{ item }">
            <v-btn
              v-if="item.in"
              :color="getColor(item.inErr)"
              dark
              @click="
                showMap = true
                goTo(item.outAddr)
              "
              >{{ item.inDate }}</v-btn
            >
          </template>
          <template v-slot:item.out="{ item }">
            <v-btn
              v-if="item.out"
              :color="getColor(item.outErr)"
              dark
              @click="
                showMap = true
                goTo(item.inAddr)
              "
              >{{ item.outDate }}</v-btn
            >
          </template> -->
          <template v-slot:item.sum="{ item }">
            <v-btn v-if="item.leave" color="info" dark>{{ item.sum }}</v-btn>
            <span v-else>{{ item.sum }}</span>
          </template>
        </v-data-table>
      </v-col>
      <!-- <v-col :cols="showMap ? 4 : 0" v-show="showMap">
        <gmap-map
          ref="mapRef"
          :center="{ lat: 18.7957392, lng: 98.9526686 }"
          :zoom="18"
          style="width: 100%; height: 70vh"
        >
          <gmap-marker :position="marker" :draggable="false" />
        </gmap-map>
      </v-col> -->
    </v-row>
  </v-card>
</template>

<script>
export default {
  props: {
    workHour: {
      type: Array,
      default: () => [],
    },
  },
  data: () => ({
    showMap: false,
    start: '',
    end: '',
    headers: [
      { text: 'เดือน/วัน/ปี', value: 'date', align: 'center' },
      { text: 'เวลาเข้า', value: 'in', align: 'center', sortable: false },
      { text: 'เวลาออก', value: 'out', align: 'center', sortable: false },
      { text: 'รวม (hr:min)', value: 'sum', align: 'center', sortable: false },
    ],
    //map
    marker: { lat: 0, lng: 0 },
  }),
  computed: {
    workHourFormated() {
      const whs = this.workHour
      var r = []
      var i = -1
      whs.forEach((wh) => {
        wh.attendance.forEach((att) => {
          console.log(this.dateFormated(new Date(att.time)) + '/' + att.type)
          if (att.type === '1') {
            i++
            r.push({
              date: wh.date,
              in: new Date(att.time),
              inDate: this.dateFormated(new Date(att.time)),
              inErr: att.isLate,
              inAddr: att.location,
              out: '',
              outDate: '',
              outErr: 1,
              outAddr: '',
              sum: '',
            })
          } else if (att.type === '2') {
            r[i].out = new Date(att.time)
            ;(r[i].outDate = this.dateFormated(new Date(att.time))),
              (r[i].outErr = att.isLate)
            r[i].outAddr = att.location
            var sum = (r[i].out - r[i].in) / 1000 / 60
            var h = this.shiftZero(sum / 60)
            var m = this.shiftZero(sum % 60)
            r[i].sum = h + ':' + m
          } else if (att.type === '3') {
            i++
            r.push({
              date: wh.date,
              sum: 'ลา',
              leave: true,
            })
          }
        })
        console.log(JSON.stringify(r[i]) + ' :' + i)
      })
      return r
    },
  },
  methods: {
    getColor: (late) => (late ? 'warning' : 'success'),
    shiftZero(m) {
      var res
      m >= 10 ? (res = `${m}`) : m > 0 ? (res = `0${m}`) : (res = '00')
      return res
    },
    dateFormated(d) {
      var m = this.shiftZero(d.getMinutes())
      var h = this.shiftZero(d.getHours())
      return `${h}:${m}`
    },
    goTo(addr) {
      this.$refs.mapRef.$mapPromise.then((map) => {
        map.panTo(addr)
      })
      this.marker = addr
    },
  },
}
</script>
<template>
  <div>
    <v-btn text @click="show = !show" block style="padding-left: 0px">
      <v-icon>{{ show ? 'mdi-chevron-up' : 'mdi-chevron-down' }}</v-icon>
      <h3>การลาของฉัน</h3>
      <v-spacer></v-spacer>
    </v-btn>
    <v-expansion-panels v-show="show">
      <v-expansion-panel
        v-for="(item, i) in myReq"
        :key="i"
        :class="reqClass(item.type)"
      >
        <v-expansion-panel-header
          disable-icon-rotate
          style="padding-right: 3vw; padding-left: 3vw"
        >
          <template v-slot:actions>
            <div
              style="
                display: flex;
                flex-wrap: wrap;
                flex-direction: row-reverse;
                width: 75px;
              "
            >
              <v-chip color="orange" dark small
                ><p style="font-size: 13px">รออนุมัติ</p></v-chip
              >
              <v-btn icon>
                <v-icon color="info">mdi-chevron-down</v-icon>
              </v-btn>
            </div>
          </template>
          <v-avatar style="flex: 0" size="35" dark :color="reqColor(item.type)">
            <v-icon dark>{{ reqIcon(item.type) }}</v-icon>
          </v-avatar>
          <div style="padding-left: 2vw; height: 7vh">
            <div style="display: flex; flex-wrap: wrap; margin-bottom: 1vh">
              <h4>{{ item.type }}</h4>
            </div>
            <div v-if="item.type === 'ลา'" class="detail">
              <p style="margin-buttom: 10px">
                ขอลาวันที่: <span>{{ formatDate(item.start) }}</span>
              </p>
              <br />
              ถึงวันที่: <span>{{ formatDate(item.end) }}</span>
            </div>
            <div v-if="item.type === 'เปลี่ยนกะ'" class="detail">
              <p style="margin-buttom: 10px">
                ขอเปลี่ยนจาก: <span>{{ item.oldSlot }}</span>
              </p>
              <br />
              เป็น:
              <span>{{ item.newSlot }}</span>
            </div>
          </div>
        </v-expansion-panel-header>
        <v-expansion-panel-content style="color: gray; font-size: 80%">
          <v-text-field
            label="เหตุผลของผู้ขอ"
            v-model="item.reason"
            readonly
            dense
          ></v-text-field>
          <v-text-field
            label="เหตุผล"
            v-model="item.confirmedReason"
            dense
          ></v-text-field>
          <v-icon small>mdi-clock</v-icon>ส่งเมื่อ:
          {{ formatDate(item.requestTime) }}
        </v-expansion-panel-content>
      </v-expansion-panel>
    </v-expansion-panels>
  </div>
</template>
<script>
export default {
  props: {
    req: {
      type: Array,
      default: () => [],
    },
  },
  data: () => ({
    show: true,
  }),
  computed: {
    myReq() {
      return JSON.parse(JSON.stringify(this.req))
    },
  },
  methods: {
    reqClass(t) {
      if (t === 'ลา') {
        return 'leave-req'
      } else if (t === 'เปลี่ยนกะ') {
        return 'change-shift-req'
      } else if (t === 'เข้าร่วมบริษัท') {
        return 'job-application-req'
      }
    },
    reqIcon(t) {
      if (t === 'ลา') {
        return 'mdi-stethoscope'
      } else if (t === 'เปลี่ยนกะ') {
        return 'mdi-calendar-clock'
      } else if (t === 'เข้าร่วมบริษัท') {
        return 'mdi-account-plus'
      }
    },
    reqColor(t) {
      if (t === 'ลา') {
        return 'warning'
      } else if (t === 'เปลี่ยนกะ') {
        return 'accent'
      } else if (t === 'เข้าร่วมบริษัท') {
        return 'error'
      }
    },
    shiftZero(m) {
      var res
      m >= 10 ? (res = `${m}`) : m > 0 ? (res = `0${m}`) : (res = '00')
      return res
    },
    formatDate(d) {
      var date = new Date(d)
      return (
        date.toLocaleString('th-TH', {
          year: 'numeric',
          month: 'long',
          day: 'numeric',
        }) +
        ' ' +
        this.shiftZero(date.getHours()) +
        ':' +
        this.shiftZero(date.getMinutes()) +
        ' น.'
      )
    },
  },
}
</script>
<style lang="scss" scoped>
@import './assets/request.scss';
</style>

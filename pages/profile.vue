<template>
  <v-card class="mx-auto" max-width="400">
    <v-img
      class="black--text align-end"
      height="200px"
      src="https://uppic.cc/d/SidqxA2eI-gcvSEANCsXo?fbclid=IwAR0fHp2c-zT_Vwtm_VTGg1KIOIzZDmgz8xrOkANqGUoaO8J-WGrC86Y2NBU"
    >
      <v-card-title
        ><h2 style="background-color: white; padding: 5px">
          ข้อมูลพนักงาน
        </h2></v-card-title
      >
    </v-img>
    <v-tabs v-model="tab">
      <v-tab v-for="item in items" :key="item">
        {{ item }}
      </v-tab>
    </v-tabs>

    <v-tabs-items v-model="tab">
      <v-tab-item>
        <v-card-text style="padding: 5% 5% 50% 5%; background-color: #f5f5f5">
          <div class="cardMobile">
            <v-row justify="space-between">
              <v-col>
                <h3 style="padding: 2vw 0vw 2vw 2vw; color: #1a237e">
                  ข้อมูลติดต่อ
                </h3>
              </v-col>
              <v-col>
                <div style="display: flex; flex-direction: row-reverse">
                  <v-btn
                    v-if="!editing"
                    dark
                    icon
                    color="info"
                    @click="editing = true"
                  >
                    <v-icon>mdi-pencil</v-icon>
                  </v-btn>
                  <v-btn
                    v-if="editing"
                    dark
                    color="success"
                    @click="saveChange"
                  >
                    บันทึก
                  </v-btn>
                  <div style="width: 1vw"></div>
                  <v-btn
                    v-if="editing"
                    outlined
                    color="error"
                    @click="deleteChange"
                  >
                    ยกเลิก
                  </v-btn>
                </div>
              </v-col>
            </v-row>

            <profile-detail
              icon="mdi-account"
              label="ชื่อ สกุล "
              :data="personalDetail.name"
              :editing="editing"
              color="light-blue darken-3"
              ref="name"
              @dataChange="
                (d) => (personalDetail.name = JSON.parse(JSON.stringify(d)))
              "
            />
            <profile-detail
              icon="mdi-phone"
              label="เบอร์โทร "
              :data="personalDetail.tel"
              :editing="editing"
              color="cyan"
              ref="tel"
              @dataChange="
                (d) => (personalDetail.tel = JSON.parse(JSON.stringify(d)))
              "
            />
            <profile-detail
              icon="mdi-cellphone-android"
              label="lineId "
              :data="personalDetail.lineId"
              :editing="editing"
              color="success"
              ref="lineId"
              @dataChange="
                (d) => (personalDetail.lineId = JSON.parse(JSON.stringify(d)))
              "
            />
            <profile-detail
              icon="mdi-email"
              label="e-mail "
              :data="personalDetail.email"
              :editing="editing"
              color="pink accent-2"
              ref="email"
              @dataChange="
                (d) => (personalDetail.email = JSON.parse(JSON.stringify(d)))
              "
            />
          </div>
          <br />
          <div class="cardMobile">
            <h3 style="padding: 2vw 0vw 2vw 2vw; color: #1a237e">
              ข้อมูลการทำงาน
            </h3>
            <profile-detail-selector
              icon="mdi-bookmark"
              label="แผนก"
              :data="personalDetail.role"
              :items="roles"
              color="info"
              ref="role"
              @dataChange="
                (d) => (personalDetail.role = JSON.parse(JSON.stringify(d)))
              "
            />
            <profile-detail
              icon="mdi-star"
              label="ตำแหน่ง "
              :data="personalDetail.pos"
              color="warning"
              ref="pos"
              @dataChange="
                (d) => (personalDetail.pos = JSON.parse(JSON.stringify(d)))
              "
            />
            <profile-detail
              icon="mdi-cube-outline"
              label="ประเภท "
              :data="personalDetail.type"
              :items="types"
              color="amber"
              ref="type"
              @dataChange="
                (d) => (personalDetail.type = JSON.parse(JSON.stringify(d)))
              "
            />
            <profile-detail
              icon="mdi-cash"
              label="เงินเดือน "
              :data="salaryFormat"
              color="success"
              ref="salary"
              @dataChange="
                (d) => (personalDetail.salary = JSON.parse(JSON.stringify(d)))
              "
            />
          </div>
          <br />
        </v-card-text>
      </v-tab-item>
      <v-tab-item>
        <v-card flat style="padding: 5% 5% 100% 5%; background-color: #f5f5f5">
          <!-- <div class="cardMobile">
          <h3>ประเภทการลาคงเหลือ</h3>
            <profile-detail
              icon="mdi-stethoscope"
              label="ลาป่วย"
              :data="sickleave + ' วัน'"
              :editing="false"
            />
            <profile-detail
              icon="mdi-sunglasses"
              label="ลากิจ"
              :data="businessleave + ' วัน'"
              :editing="false"
            />
          <div class="cardMobile">
            <profile-detail
              icon="mdi-calendar-text"
              label="วันลาคงเหลือ"
              :data="remainingleave + ' วัน'"
              :editing="false"
            />
          </div>
          </div> -->
          <h3>สิทธิ์วันลา</h3>
          <v-data-table
            :headers="headers"
            :items="day"
            mobile-breakpoint
            hide-default-footer
          >
            <template v-slot:item.name="{ item }">
              <h3>
                {{ item.name }}
              </h3>
            </template>
            <template v-slot:item.used="{ item }">
              <v-chip>
                {{ item.used }}
              </v-chip>
            </template>
            <template v-slot:item.balance="{ item }">
              <v-chip color="success" dark>
                {{ item.balance }}
              </v-chip>
            </template>
          </v-data-table>
          <br />
          <leave-history :req="req" />
        </v-card>
      </v-tab-item>
    </v-tabs-items>
  </v-card>
</template>

<script>
import LeaveHistory from '~/components/profile/leaveHistory.vue'
import profileDetail from '~/components/profile/profileDetail'
export default {
  components: {
    profileDetail,
    LeaveHistory,
  },
  data: () => ({
    editing: false,
    personalDetail: {
      name: 'สมใจ ใฝ่ทำงาน',
      role: 'พนักงานบัญชี',
      pos: 'พนักงานขาย',
      type: 'พนักงานประจำ',
      salary: '20,000',
      tel: '095111111',
      lineId: 'lnwZa007',
      email: 'lnw@mail.com',
    },
    roles: ['พนักงานบัญชี', 'พนักงานขาย', 'พนักงานยกของ'],
    types: ['พนักงานประจำ', 'พนักงานรายวัน'],
    remainingleave: '30', //วันลาคงเหลือ
    sickleave: '7',
    businessleave: '10',
    tab: null,
    selection: 1,
    items: ['ข้อมูลส่วนตัว', 'วันลา'],
    headers: [
      {
        text: 'ประเภทการลา',
        align: 'start',
        sortable: false,
        value: 'name',
      },
      { text: 'ใช้แล้ว', value: 'used' },
      { text: 'คงเหลือ', value: 'balance' },
    ],
    day: [
      {
        name: 'ลาป่วย',
        used: 2,
        balance: 18,
      },
      {
        name: 'ลากิจ',
        used: 0,
        balance: 15,
      },
      {
        name: 'ลาพักร้อน',
        used: 3,
        balance: 14,
      },
    ],
    req: [
      {
        type: 'ขอลากิจ',
        time: 'วันพุธ 28 ตุลาคม 2563, 09:30',
      },
      {
        type: 'ขอลาป่วย',
        time: 'วันพุธ 28 ตุลาคม 2563, 09:30',
      },
      {
        type: 'ขอเปลี่ยนกะ',
        time: 'วันพุธ 28 ตุลาคม 2563, 09:30',
      },
    ],
  }),
  computed: {
    salaryFormat: function () {
      if (this.personalDetail.type == 'พนักงานประจำ') {
        return this.personalDetail.salary + ' บาท / เดือน'
      } else if (this.personalDetail.type == 'พนักงานรายวัน') {
        return this.personalDetail.salary + ' บาท / ชั่วโมง'
      }
    },
  },
  methods: {
    reserve() {
      this.loading = true
      setTimeout(() => (this.loading = false), 2000)
    },
    saveChange() {
      this.editing = false
      this.$refs.name.saveData()
      this.$refs.role.saveData()
      this.$refs.pos.saveData()
      this.$refs.type.saveData()
      this.$refs.salary.saveData()
      this.$refs.tel.saveData()
      this.$refs.lineId.saveData()
      this.$refs.email.saveData()
    },
    deleteChange() {
      this.editing = false
      this.editingPersonalDetail = JSON.parse(
        JSON.stringify(this.personalDetail)
      )
    },
  },
  mounted() {
    this.deleteChange()
  },
}
</script>

<style lang="scss" scoped>
p {
  margin: 1.5% 1.5% !important;
}
.hide-btn {
  overflow: hidden;
  max-height: 0px !important;
  min-height: 0px !important;
  max-width: 0px !important;
  min-width: 0px !important;
  padding: 0%;
  border: none;
}
h3 {
  margin-bottom: 3px;
}
.cardMobile {
  border: solid #bcaaa4 0.8px;
  border-radius: 5px; //ขอบมน
  padding: 3px;
  background-color: #ffffff;
}
</style>
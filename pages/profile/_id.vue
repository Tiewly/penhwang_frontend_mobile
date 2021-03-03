<template>
  <v-card class="mx-auto" max-width="400">
    <v-img
      class="black--text align-end"
      height="200px"
      src="https://sv1.picz.in.th/images/2021/03/02/oG7jvk.png?fbclid=IwAR1zzXKz-uvXjt6w3bAzeADRcHsqQvS068j2j5Bq2-Nnqnj0MNuVmvbX6Jg"
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

            <!-- <profile-detail
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
            /> -->
            <Details ref="contact" :editing="editing" :items="contact" />
          </div>
          <br />
          <div class="cardMobile">
            <h3 style="padding: 2vw 0vw 2vw 2vw; color: #1a237e">
              ข้อมูลการทำงาน
            </h3>
            <Details ref="personal" :items="personal" />
            <!-- <profile-detail-selector
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
            /> -->
          </div>
          <br />
        </v-card-text>
      </v-tab-item>
      <v-tab-item>
        <v-card flat style="padding: 5% 5% 70% 5%; background-color: #f5f5f5">
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
import { mapGetters } from 'vuex'
import ProfileDetail from '~/components/profile/profileDetail.vue'
import Details from '~/components/Details.vue'
export default {
  components: {
    profileDetail,
    LeaveHistory,
    ProfileDetail,
    Details,
  },
  data: () => ({
    editing: false,
    tab: 0,
    roles: ['พนักงานบัญชี', 'พนักงานขาย', 'พนักงานยกของ'],
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
        type: 'ลา',
        time: 'วันพุธ 28 ตุลาคม 2563, 09:30',
      },
      {
        type: 'ลา',
        time: 'วันพุธ 28 ตุลาคม 2563, 09:30',
      },
      {
        type: 'เปลี่ยนกะ',
        oldSlot: 'กะเช้าของคนโสด',
        newSlot: 'กะเช้าของคนมีคู่',
      },
    ],
  }),
  computed: {
    ...mapGetters({
      employeeGetter: 'employee/getEmployeeById',
      types: 'employee/getTypes',
    }),
    employee() {
      return this.employeeGetter(this.$route.params.id)
    },
    personal() {
      return [
        {
          att: 'role',
          icon: 'mdi-bookmark',
          label: 'แผนก ',
          data: this.employee.role,
          color: 'info',
          type: 'select',
          items: this.roles,
        },
        {
          att: 'pos',
          icon: 'mdi-star',
          label: 'ตำแหน่ง ',
          data: this.employee.pos,
          color: 'warning',
          type: 'norm',
        },
        {
          att: 'type',
          icon: 'mdi-cube-outline',
          label: 'ประเภท ',
          data: this.employee.salary.type,
          color: 'amber',
          type: 'select',
          items: this.types,
        },
        {
          att: 'amount',
          icon: 'mdi-cash',
          label: 'เงินเดือน ',
          data: this.employee.salary.amount,
          color: 'success',
          type: 'norm',
        },
      ]
    },
    contact() {
      return [
        {
          att: 'name',
          icon: 'mdi-account',
          label: 'ชื่อ-สกุล ',
          data: this.employee.name,
          color: 'light-blue darken-3',
          type: 'norm',
        },
        {
          att: 'tel',
          icon: 'mdi-phone',
          label: 'เบอร์ติดต่อ ',
          data: this.employee.tel,
          color: 'cyan',
          type: 'norm',
        },
        {
          att: 'lineId',
          icon: 'mdi-cellphone-android',
          label: 'lineId ',
          data: this.employee.lineId,
          color: 'success',
          type: 'norm',
        },
        {
          att: 'email',
          icon: 'mdi-email',
          label: 'อีเมล ',
          data: this.employee.email,
          color: 'pink accent-2',
          type: 'norm',
        },
      ]
    },
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
      const x = this.$refs.contact.saveData()
      // TODO push x to store
      this.editing = false
    },
    deleteChange() {
      this.editing = false
    },
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
<template>
  <div>
    <v-toolbar flat color="#6A76AB" dark>
      <h2>ข้อมูลพนักงาน</h2>
      <v-spacer></v-spacer>
      <template v-slot:extension>
        <v-tabs v-model="tab">
          <v-tab v-for="item in items" :key="item">
            {{ item }}
          </v-tab>
        </v-tabs>
      </template>
    </v-toolbar>

    <v-tabs-items v-model="tab">
      <v-tab-item>
        <v-card flat>
          <v-row no-gutters style="padding: 5% 7% 0 2%">
            <v-col cols="10">
              <v-form v-model="valid">
                <profile-detail
                  icon="mdi-account"
                  label="ชื่อ-สกุล"
                  :data="name"
                  :editing="editing"
                  ref="employeeName"
                  @dataChange="(d) => (name = JSON.parse(JSON.stringify(d)))"
                />
                <profile-detail
                  icon="mdi-phone"
                  label="เบอร์โทรศัพท์"
                  :data="tel"
                  :editing="editing"
                  ref="employeeTel"
                  @dataChange="(d) => (tel = JSON.parse(JSON.stringify(d)))"
                />
                <profile-detail
                  icon="mdi-email"
                  label="อีเมล"
                  :data="email"
                  :editing="editing"
                  ref="employeeMail"
                  @dataChange="(d) => (email = JSON.parse(JSON.stringify(d)))"
                />
              </v-form>
            </v-col>

            <v-col cols="2">
              <div class="text-right">
                <v-btn
                  right
                  outlined
                  icon
                  color="indigo"
                  :class="editing ? 'hide-btn' : ''"
                  @click="editing = true"
                  ><v-icon>mdi-pencil</v-icon>
                </v-btn>
                <v-btn
                  outlined
                  :icon="!editing"
                  color="success"
                  small
                  :class="editing ? '' : 'hide-btn'"
                  @click="
                    editing = false
                    saveData()
                  "
                  ><span>บันทึก</span>
                </v-btn>
                <v-space></v-space>
                <v-btn
                  outlined
                  color="red"
                  small
                  class="my-2"
                  :class="editing ? '' : 'hide-btn'"
                  @click="editing = false"
                  >ยกเลิก</v-btn
                >
              </div>
            </v-col>
            <v-col cols="12">
              <v-form v-model="valid">
                <employee-address
                  :address="address"
                  :editing="editing"
                  ref="employeeAddr"
                  @dataChange="(d) => (address = JSON.parse(JSON.stringify(d)))"
                />
              </v-form>
            </v-col>
          </v-row>
        </v-card>
      </v-tab-item>
      <v-tab-item>
        <v-card flat style="padding: 5% 5% 0 5%">
          <h3>ข้อมูลการทำงาน</h3>
          <div class="cardMobile">
            <profile-detail
              icon="mdi-city"
              label="แผนก"
              :data="department"
              :editing="false"
            />
            <profile-detail
              icon="mdi-clipboard-account"
              label="ตำแหน่ง"
              :data="position"
              :editing="false"
            />
          </div>
          <br />
          <h3>ข้อมูลเงินเดือน</h3>
          <div class="cardMobile">
            <profile-detail
              icon="mdi-package-variant-closed"
              label="ประเภทพนักงาน"
              :data="type"
              :editing="false"
            />
            <profile-detail
              icon="mdi-cash"
              label="เงินเดือน"
              :data="salaryFormat"
              :editing="false"
            />
          </div>
        </v-card>
      </v-tab-item>
      <v-tab-item>
        <v-card flat style="padding: 5% 5% 0 5%">
          <h3>สิทธิ์วันลา</h3>
          <div class="cardMobile">
            <profile-detail
              icon="mdi-calendar-text"
              label="วันลาคงเหลือ"
              :data="remainingleave + ' วัน'"
              :editing="false"
            />
          </div>
          <br />
          <h3>ประเภทการลาคงเหลือ</h3>
          <div class="cardMobile">
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
          </div>
        </v-card>
      </v-tab-item>
    </v-tabs-items>
  </div>
</template>

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
}
</style>

<script>
import ProfileDetail from '~/components/ProfileDetail.vue'
import EmployeeAddress from '~/components/EmployeeAddress.vue'
export default {
  components: { ProfileDetail, EmployeeAddress },
  data: () => ({
    valid: false,
    editing: false,
    name: 'สมศรี ใจรักเธอ',
    tel: '0811111111',
    email: 'example@mail.com',
    address: {
      building: '30 ปีที่มีนักศึกษาดี', //ตึก
      room: '422', //ห้อง
      floor: '4', //ชั้น
      houseNo: '239', //บ้านเลขที่
      villageNo: '', //หมู่ที่
      lane: '', //ซอย
      road: 'ห้วยแก้ว', //ถนน
      subDistrict: 'สุเทพ', //ตำบล
      district: 'เมือง', //อำเภอ
      province: 'เชียงใหม่', //จังหวัด
      postalCode: '50200', //รหัสไปรษณีย์
    },
    department: 'ทำความสะอาด', //แผนก
    position: 'พนักงานทำความสะอาด', //ตำแหน่ง
    type: 'Full-time', //ประเภทพนักงาน
    salary: '20,000',
    remainingleave: '30', //วันลาคงเหลือ
    sickleave: '7',
    businessleave: '10',
    tab: null,
    selection: 1,
    items: ['ข้อมูลส่วนตัว', 'บริษัท', 'วันลา'],
  }),
  computed: {
    salaryFormat: function () {
      if (this.type == 'Full-time') {
        return this.salary + ' บาท / เดือน'
      } else if (this.type == 'Part-time') {
        return this.salary + ' บาท / ชั่วโมง'
      }
    },
  },
  methods: {
    reserve() {
      this.loading = true
      setTimeout(() => (this.loading = false), 2000)
    },
    saveData() {
      this.$refs.employeeName.saveData()
      this.$refs.employeeMail.saveData()
      this.$refs.employeeTel.saveData()
      this.$refs.employeeAddr.saveData()
    },
  },
}
</script>
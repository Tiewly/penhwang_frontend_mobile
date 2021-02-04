<template>
  <v-card class="mx-auto" max-width="400">
    <v-img
      class="black--text align-end"
      height="200px"
      src="https://uppic.cc/d/stk2IrWlgLJT5RWJAS96D?fbclid=IwAR0lIBN_QuCuTLhzLBOHpANq3am_1Iwzxn7igVmLws9DBvjUMpbITRbtmYY"
    >
      <v-card-title
        ><h2 style="background-color: white; padding: 5px">
          แบบฟอร์มเข้าบริษัท
        </h2></v-card-title
      >
    </v-img>

    <div style="background-color: #f5f5f5; height: 100%">
      <v-form v-model="valid" lazy-validation style="padding: 5% 5% 5% 5%">
        <h3 style="color: #6d4c41">
          <v-icon left style="color: #ff6f00"> mdi-key </v-icon>Passcode
          ของบริษัทของคุณ
        </h3>
        <i style="color: #757575">ติดต่อกับผู้ดูแลของคุณเพื่อรับรหัสผ่าน</i>
        <v-textarea
          v-model="passcode"
          :rules="passcodeRules"
          required
          auto-grow
          outlined
          rows="2"
          row-height="20"
          hide-details
          background-color="#FFFFFF"
        ></v-textarea>
        <br />

        <h3>ชื่อ :</h3>
        <i style="color: #757575">ไม่ต้องใส่คำนำหน้าชื่อ</i>
        <v-textarea
          v-model="firstname"
          :rules="firstnameRules"
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

        <h3>นามสกุล :</h3>
        <v-textarea
          v-model="lastname"
          :rules="lastnameRules"
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

        <h3>เบอร์โทรศัพท์ :</h3>
        <v-textarea
          v-model="tel"
          :rules="telRules"
          required
          append-icon="mdi-phone"
          auto-grow
          dense
          outlined
          rows="2"
          row-height="20"
          hide-details
          background-color="#FFFFFF"
        ></v-textarea>
        <br />

        <h3>Line ID :</h3>
        <v-textarea
          v-model="lineId"
          :rules="lineIdRules"
          required
          append-icon="mdi-cellphone"
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
        <!-- <h3>test</h3>
    <p>date1: {{date1}}</p>
    <p>date2: {{date2}}</p>
    <p>time1: {{time1}}</p>
    <p>time2: {{time2}}</p> -->
      </v-form>
    </div>
  </v-card>
</template>

<script>
export default {
  data: (vm) => ({
    valid: false,
    passcode: null,
    passcodeRules: [(v) => !!v || 'Passcode is required'],

    firstname: null,
    firstnameRules: [(v) => !!v || 'First name is required'],

    lastname: null,
    lastnameRules: [(v) => !!v || 'Last name is required'],

    tel: null,
    telRules: [(v) => !!v || 'Phone number  is required'],

    lineId: null,
    lineIdRules: [(v) => !!v || 'Line ID is required'],
  }),
}
</script>


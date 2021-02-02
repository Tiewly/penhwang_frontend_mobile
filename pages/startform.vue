<template>
  <div style="background-color: #f5f5f5; height: 100%">
    <v-form v-model="valid" lazy-validation style="padding: 5% 5% 5% 5%">
      <h2>แบบฟอร์มเข้าบริษัท</h2>
      <br />
      <h3 style="color: #6d4c41">
        <v-icon left> mdi-key </v-icon>Passcode ของบริษัทของคุณ
      </h3>
      <i style="color: #bdbdbd">ติดต่อกับผู้ดูแลของคุณเพื่อรับรหัสผ่าน</i>
      <v-textarea
        v-model="passcode"
        :rules="passcodeRules"
        required
        auto-grow
        outlined
        rows="2"
        row-height="20"
      ></v-textarea>

      <h3>ชื่อ :</h3>
      <v-textarea
        v-model="firstname"
        :rules="firstnameRules"
        required
        auto-grow
        dense
        outlined
        rows="2"
        row-height="20"
      ></v-textarea>

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
      ></v-textarea>

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
      ></v-textarea>

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
      ></v-textarea>

      <v-btn
        :disabled="!valid"
        @click="validate"
        block
        color="primary"
        nuxt
        to="/inspire"
        large
      >
        <h3>ส่งแบบฟอร์ม</h3>
      </v-btn>
      <!-- <h3>test</h3>
    <p>date1: {{date1}}</p>
    <p>date2: {{date2}}</p>
    <p>time1: {{time1}}</p>
    <p>time2: {{time2}}</p> -->
    </v-form>
  </div>
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


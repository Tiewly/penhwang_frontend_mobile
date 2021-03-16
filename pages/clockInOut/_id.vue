<template>
  <v-container fill-height>
    <v-layout column align-center justify-center>
      <v-divider vertical inset />
      <h1>{{ time }}</h1>
      <h2>{{ now }}</h2>
      <br />
      <v-divider vertical inset />
      <v-dialog v-model="dialog" persistent max-width="290">
        <template v-slot:activator="{ on, attrs }">
          <v-btn
            color="error"
            fab
            large
            dark
            v-bind="attrs"
            v-on="on"
            style="padding: 60px 60px 60px 60px; margin-bottom: 20px"
          >
            <v-icon>mdi-alarm</v-icon>
          </v-btn>
        </template>
        <v-card style="padding: 7% 0% 3% 0%">
          <v-card-text
            class="headline"
            style="text-align: center; color: black"
          >
            บันทึกสำเร็จ!
          </v-card-text>
          <v-card-text style="text-align: center"
            >08/02/2021 08:29:20</v-card-text
          >
          <v-card-actions>
            <v-btn
              block
              large
              color="green darken-1"
              @click="dialog = false"
              class="white--text"
            >
              ตกลง
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
      <h3 style="text-align: center">กดเพื่อบันทึกเวลาทำงาน</h3>
      <v-divider vertical inset />
    </v-layout>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    now: null,
    time: null,
    dialog: false,
  }),
  created() {
    setInterval(() => {
      const date = new Date()
      ;(this.now = date.toLocaleString('th-TH', {
        year: 'numeric',
        month: 'long',
        day: 'numeric',
      })),
        (this.time =
          date.getHours() +
          ':' +
          date.getMinutes() +
          ':' +
          date.getSeconds() +
          ' น.')
    }, 1000)
  },
}
</script>
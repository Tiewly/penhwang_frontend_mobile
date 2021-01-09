<template>
  <div class="text-center">
    <div v-if="canClock">
      <h1 class="mdi mdi-emoticon-excited" />
      <br>
      <v-btn
        dark
        elevation="5"
        color="indigo"
        @click="clockIn"
      >Clock In</v-btn>
    </div>
    <div v-else>
      <h1 class="mdi mdi-emoticon-sad" />
    </div>
    <div>
      <h1>test data</h1>
      <p> {{JSON.stringify(sendData)}}</p>
      <p>{{responseFromClockReq}}</p>
    </div>
  </div>
</template>
<script>
export default {
  data:()=>({
    backendUrl: 'https://us-central1-penhwang-d350c.cloudfunctions.net/',
    canClock: false,
    sendData: {
      location: {latitude: 0, longitude: 0},
      employeeId: "",
      spotName: ""
    },
    responseFromClockReq: {}
  }),
  methods: {
    getLocation() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(this.showPosition)
      } else {
        x.innerHTML = "Geolocation is not supported by this browser."
      }
    },
    showPosition(position) {
      this.sendData.location = {
        latitude: position.coords.latitude,
        longitude: position.coords.longitude
      }
    },
    async clockReq() {
      const ip = await this.$axios.$post(this.backendUrl+'clockReq', this.sendData)
      if(ip.isSuccess){
        this.canClock = ip.data.pass
        if(ip.data.pass){
          this.sendData.spotName = ip.data.name
          alert(':) You can Clock In here. :)')
        }else{
          alert(`You can't Clock In here.`)
        }
      }else{
        console.log(ip)
      }
      return { ip }
    },
    async clockIn() {
      const res = await this.$axios.$post(this.backendUrl+'clockIn', this.sendData)
      if(res.isSuccess){
        alert(res.message)
      }
    }
  },
  async mounted() {
    await this.getLocation()
    await (this.sendData.employeeId = this.$route.query.employeeId)
    console.log(this.sendData)
    this.responseFromClockReq = await this.clockReq()
  }
}
</script>

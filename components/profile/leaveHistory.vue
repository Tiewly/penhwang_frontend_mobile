<template>
  <div>
    <v-btn text @click="show = !show" block style="padding-left: 0px">
      <v-icon>{{ show ? 'mdi-chevron-up' : 'mdi-chevron-down' }}</v-icon>
      <h3>การลาของฉัน</h3>
      <v-spacer></v-spacer>
    </v-btn>
    <v-expansion-panels v-show="show">
      <v-expansion-panel
        v-for="(item, i) in req"
        :key="i"
        :class="getReqClass(item.type)"
      >
        <v-expansion-panel-header disable-icon-rotate>
          <template v-slot:actions>
            <div
              style="
                display: flex;
                flex-wrap: wrap;
                flex-direction: row-reverse;
              "
            >
              <v-btn color="orange" dark>รออนุมัติ</v-btn>
              <div style="width: 1vw"></div>
              <v-btn color="info" dark outlined> ดูรายละเอียด</v-btn>
            </div>
            <!-- <v-tooltip top>
              <template v-slot:activator="{ on, attrs }">
                <v-btn icon small v-bind="attrs" v-on="on">
                  <v-icon color="info">mdi-eye</v-icon>
                </v-btn>
              </template>
              <span>ดูรายละเอียด</span>
            </v-tooltip> -->
            <!-- <v-tooltip top>
              <template v-slot:activator="{ on, attrs }">
                <v-btn icon small v-bind="attrs" v-on="on">
                  <v-icon color="success">mdi-thumb-up</v-icon>
                </v-btn>
              </template>
              <span>อนุญาต</span>
            </v-tooltip> -->
          </template>
          <div style="padding-left: 1vw">
            <div style="display: flex; flex-wrap: wrap">
              <p>{{ item.type }}</p>
              <span>{{ item.name }}</span>
            </div>
            <div
              style="
                color: gray;
                font-size: 80%;
                display: flex;
                flex-wrap: wrap;
              "
            >
              <p><v-icon small>mdi-clock</v-icon>วันที่ขอ:</p>
              {{ item.time }}
            </div>
          </div>
        </v-expansion-panel-header>
        <v-expansion-panel-content
          v-if="item.type === 'ขอลากิจ' || item.type === 'ขอลาป่วย'"
        >
          ขอลาวันที่: <span>1 กพ 2564</span> ถึงวันที่: <span>2 กพ 2564</span>
        </v-expansion-panel-content>
        <v-expansion-panel-content v-if="item.type === 'ขอเปลี่ยนกะ'">
          ขอเปลี่ยนจาก: <span>กะเช้า</span> เป็น: <span>กะสาย</span>
        </v-expansion-panel-content>
      </v-expansion-panel>
    </v-expansion-panels>
  </div>
</template>
<script>
export default {
  props: ['req'],
  data: () => ({
    show: true,
  }),
  methods: {
    getReqClass(t) {
      if (t === 'ขอลากิจ') {
        return 'bus-leave-req'
      } else if (t === 'ขอลาป่วย') {
        return 'sick-leave-req'
      } else if (t === 'ขอเปลี่ยนกะ') {
        return 'change-shift-req'
      }
    },
  },
}
</script>
<style lang="scss" scoped>
.v-expansion-panel-header {
  color: inherit;
  p {
    margin: 0;
  }
  span {
    color: #6d4c41;
    margin-left: 10px;
    font-size: 1.25rem;
  }
}
.v-expansion-panel-content {
  color: #757575;
}
.sick-leave-req {
  border-top: solid #ffb300;
  color: #ffb300 !important;
  .v-expansion-panel-content {
    span {
      background-color: #ffb300;
      color: white;
      padding: 0px 10px;
      border-radius: 5px;
    }
  }
}
.bus-leave-req {
  border-top: solid #ff7043;
  color: #ff7043 !important;
  .v-expansion-panel-content {
    span {
      background-color: #ff7043;
      color: white;
      padding: 0px 10px;
      border-radius: 5px;
    }
  }
}
.change-shift-req {
  border-top: solid #78909c;
  color: #78909c !important;
  .v-expansion-panel-content {
    span {
      background-color: #78909c;
      color: white;
      padding: 0px 10px;
      border-radius: 5px;
    }
  }
}
</style>

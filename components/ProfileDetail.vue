<template>
  <div class="ProfileDetail">
    <v-icon>{{ icon }}</v-icon>
    <div v-if="!editing">
      <p>{{ label }} :</p>
      <h4>{{ data }}</h4>
    </div>
    <v-text-field
      v-model="mydata"
      :label="label"
      required
      v-else
    ></v-text-field>
  </div>
</template>
<script>
export default {
  props: ['icon', 'label', 'editing', 'data'],
  data: () => ({
    mydata: null,
  }),
  methods: {
    saveData() {
      this.$emit('dataChange', this.mydata)
    },
  },
  mounted() {
    this.mydata = JSON.parse(JSON.stringify(this.data))
  },
}
</script>
<style lang="scss" scoped>
.ProfileDetail {
  display: flex;
  align-items: center;
  padding: 1%;
  transition: 0.2s;
  margin-bottom: 2%;
  p,
  .v-input {
    color: gray;
    margin: 1.5% 1.5% !important;
  }
  p {
    min-width: 50px;
  }
  h4 {
    margin: 1.5% !important;
  }
  div {
    display: flex;
    width: 100%;
  }
}
</style>

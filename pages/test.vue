<template>
  <div>
    <a v-if="!inProgess" @click="startOrder">start</a>
    <a v-else @click="finishOrder">finish</a>
  </div>
</template>
<script>
export default {
  data: () => ({
    inProgess: false,
    //...
  }),
  methods: {
    startOrder() {
      this.inProgess = true
      //...
    },
    finishOrder() {
      this.inProgess = false
      //...
    },
  },
}
</script>

<template>
  <div>
    <v-btn @click="test">click me</v-btn>
    {{value}}
  </div>
</template>
<script>
export default {
  data: () => ({
    value: []
  }),
  methods: {
    async test(){
      const a = [1,2,3,4,5];
      this.value = a;
      let b = [];
      a.forEach(x => b.push({a:x, b:this.getX(x)}));
      const c = await Promise.all(b);
      this.value = c;
    },
    async getX(x){
      setTimeout(()=>x, 1000)
    }
  },
}
</script>

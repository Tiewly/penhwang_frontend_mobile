<template>
<div>
  <DateTimeInput
    header="Start Date Time"
    @onTimeChange="t => time = t"
    @onDateChange="d => date = d"
  />
  <h1>this is out of component</h1>
  <p>date: {{date}}</p>
  <p>time: {{time}}</p>
</div>
</template>
<script>
export default {
  data: () => ({
    date: null,
    time: null,
  })
}
</script>

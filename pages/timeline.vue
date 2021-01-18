<template>
  <div>
    <h2>ประวัติการลงเวลา</h2>
    <v-simple-table>
      <template v-slot:default>
        <thead>
          <tr>
            <th class="text-left">วัน</th>
            <th class="text-left">เวลาเข้างาน</th>
            <th class="text-left">เวลาออกงาน</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in dataDate" :key="item.date">
            <td>{{ item.date }}</td>
            <td>{{ item.timeIn }}</td>
            <td>{{ item.timeOut }}</td>
          </tr>
        </tbody>
      </template>
    </v-simple-table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dataDate: [
        {
          date: 'ศุกร์ 1',
          timeIn: 8.28,
          timeOut: 16.25,
        },
        {
          date: 'จันทร์ 4',
          timeIn: 8.39,
          timeOut: 16.45,
        },
      ],
    }
  },
}
</script>
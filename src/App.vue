<script setup>
import { RouterLink, RouterView } from 'vue-router'
import { reactive, ref } from 'vue'
import TableComponent from "./components/table.vue";

let activities = reactive([])
let activitiesExist = localStorage.getItem('activities_debola')
if(activitiesExist){
  Object.assign(activities, JSON.parse(activitiesExist)) //ganti nilai dengan object assign supaya reactive tetap berjalan
}else{
   Object.assign(activities, []) //ganti nilai dengan object assign supaya reactive tetap berjalan
}




</script>

<template>
  <div class="container">
    <RouterView :add="add" :activities="activities"/>
  </div>
</template>

<style scoped>

</style>

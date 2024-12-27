<script setup>
    import FormComponent from "./Form.vue";
    import HeaderComponent from "./Header.vue";
    import { defineProps } from 'vue'
    
    let {add, activities} = defineProps({
        add : Function,
        activities : Array
    })

    function deleteActivity(id){
        let confimation = confirm('Anda yakin ingin menghapus kegiatan ini?')
        if(!confimation)return false

        let activitiesExist = activities.filter(activity => activity.id != id)
        activities.splice(0, activities.length, ...activitiesExist);
        localStorage.setItem('activities_debola', JSON.stringify(activitiesExist))
    }

    function doneActivity(id){
        let confimation = confirm('Anda yakin ingin menyelesaikan kegiatan ini?')
        if(!confimation)return false

        let activity = activities.find(a => a.id == id)
        if( activity){
            activity.telah_selesai = 'ya'
        }else{
            alert('terjadi kesalahan ketika mencari aktivitas yang ingin diselesaikan')
        }
        localStorage.setItem('activities_debola', JSON.stringify(activities))
    }
</script>
<template>
    <div class="card p-2">
        <HeaderComponent />
        <FormComponent :add="add" :activities="activities" />
        <table class="table">
            <thead>
                <tr>
                    <th scope="col">#</th>
                    <th scope="col">Nama Kegiatan</th>
                    <th scope="col">Deskripsi</th>
                    <th scope="col">Tanggal Dilakukan</th>
                    <th scope="col">Tanggal Dibuat</th>
                    <th scope="col" class="w-25">Aksi</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td colspan="6">
                        <p align="center" v-if="activities.length == 0">Anda belum menambahkan kegiatan apapun</p>
                    </td>
                </tr>
                <tr v-for="(activity, index) in activities" :key="index">
                    <th scope="row">{{index + 1}}</th>
                    <td>{{activity.nama}}</td>
                    <td>{{activity.deskripsi}}</td>
                    <td>{{activity.tanggal_dilakukan.replace('T', ' ')}}</td>
                    <td>{{activity.tanggal_dibuat.replace('T', ' ')}}</td>
                    <td class=" d-flex justify-content-between align-items-center">
                        <button type="button" class="btn btn-primary" v-if="activity.telah_selesai == 'tidak'" @click="doneActivity(activity.id)">Selesaikan</button>
                        <p class="badge d-inline bg-primary " disabled v-if="activity.telah_selesai == 'ya'">Telah Selesai</p>
                        <button type="button" class="btn btn-outline-danger" @click="deleteActivity(activity.id)">Hapus</button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>
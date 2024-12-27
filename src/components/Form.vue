<script setup>
    import { defineProps, ref, onMounted } from 'vue'
    let {addFuntion, activities} = defineProps({
        add : Function,
        activities : Array
    })

    let nama = ref(null)
    let deskripsi = ref(null)
    let tanggal = ref(null)


    function add() {
        if(!nama.value || !deskripsi.value || !tanggal.value){
            alert('kesalahan pada sistem : form tidak te-render')
            return false
        }

        if(!nama.value.value || !deskripsi.value.value || !tanggal.value.value ){
            alert('ada kolom form yang tidak diisi')
            return false
        }

        let today = new Date().toISOString().substring(0,16);
        if(tanggal.value.value <= today){
            alert('Tanggal harus lebih dari hari ini. Tidak boleh tanggal kemarin')
            return false
        }

        let data = {
            id : Math.round(Math.random() * 10000),
            nama : nama.value.value,
            deskripsi : deskripsi.value.value,
            tanggal_dibuat : today,
            tanggal_dilakukan : tanggal.value.value,
            telah_selesai : 'tidak',
        }
        
        activities.unshift(data)
        localStorage.setItem('activities_debola', JSON.stringify(activities))

        nama.value.value = ''
        deskripsi.value.value = ''
        tanggal.value.value = today
    }

    onMounted(() => {
        let today = new Date();
        
        tanggal.value.value = today.toISOString().substring(0,16);
    })

</script>
<template>
    <div class="form my-2">
        <div class="input-group mb-3">
            <input type="text" class="form-control" ref="nama" placeholder="Nama Kegiatan" aria-label="Nama Kegiatan" aria-describedby="basic-addon1">
            <input type="text" class="form-control" ref="deskripsi" placeholder="Deskripsi" aria-label="Deskripsi" aria-describedby="basic-addon1">
            <input type="datetime-local" class="form-control" ref="tanggal" placeholder="Tanggal Dilakukan" aria-label="Tanggal" aria-describedby="basic-addon1">
            <button type="button" class="btn btn-primary" @click="add()">tambah</button>
        </div>
    </div>    
</template>
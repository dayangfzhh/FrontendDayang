<template>
  <div class="container-scroller">
    <div class="container-fluid page-body-wrapper">
      <sidebar-component></sidebar-component>
      <navbar-component></navbar-component>
      <div class="main-panel">
        <div class="col-lg-12">
      
        <div class="content-wrapper">
          <div class="row">
            <div class="col-lg-12 grid-margin stretch-card">
              <div class="card">
                  <div class="card-body">
                    <h2 class="card-title">Outlet's data</h2>
                    <router-link to="/outlet/tambah" class="d-flex flex-row-reverse">
                    <div class="btn btn-primary btn-icon-split btn-rounded">
                      <div class="mx-1">
                        <span class="icon text-white-50">
                        <i class="mdi mdi-plus"></i>
                        </span>
                        Add
                      </div>
                    </div>
                    </router-link>
                    <div class="table-responsive">
                      <table class="table">
                      <thead>
                      <tr>
                        <tr>
                          <th>#</th>
                          <th>Nama</th>
                          <th>Alamat</th>
                          <th>Aksi</th>
                        </tr>
                        </thead>
                        <tbody>
                        <tr v-for="(o, index) in outlet" :key="index">
                          <td>{{ index + 1 }}</td>
                          <td>{{ o.nama_outlet }}</td>
                          <td>{{ o.alamat }}</td>
                          <td>
                        <button type="button" class="btn btn-warning btn-rounded btn-icon">
                          <router-link :to="{name : 'editoutlet' ,params :{id : o.id_outlet }}">
                        <i class="mdi mdi-border-color"></i>
                        </router-link>
                        </button>
                            <button type="button" @click="hapus(o.id_outlet)" class="btn btn-danger btn-rounded btn-icon">
												<i class="mdi mdi-delete"></i>
												</button>
                          </td>
                        </tr>
                        </tbody>
                        </table>
                      </div>
                    </div>
              </div>
            </div>
            
          
        </div>
    </div>
  
        <footer-component></footer-component>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
    data() {
        return {
            outlet : {}
        }
    },
    created() {
        var data = JSON.parse(this.$store.state.datauser)
        var level = data.level
        if(level == 'kasir' || level == 'owner')
        {
            this.$swal("Anda tidak dapat mengakses halaman ini")
            this.$router.push('/') 
        }
        
        this.axios.get('http://127.0.0.1:8000/api/outlet', { headers : { 'Authorization' : 'Bearer ' + this.$store.state.token} })
                  .then( res => {
                      this.outlet = res.data
                  })
    },
    methods : {
      hapus(id_outlet) {
           this.$swal({
               title: "Hapus Data outlet",
               text: "Apakah anda yakin menghapus data outlet ini?",
               icon: 'waning',
               showDenyButton: true,
               //showCancelButton: "false",
               confirmButtonText: "Ya",
               denyButtonText: "Tidak",
           }).then((result) => {
               console.log(result)
               if(result.isConfirmed){
                 this.axios
                        .delete(`http://127.0.0.1:8000/api/outlet/${id_outlet}`, {
                        headers: { Authorization: "Bearer " + this.$store.state.token },
                        })
                        .then((res) => {
                        if(res.data.success) {
                        this.$swal("Sukses", res.data.message, "success")
                        let i = this.outlet.map(item => item.id_outlet).indexOf(id_outlet);
        this.outlet.splice(i, 1)
                        this.getData()
                        }
                    })
               } else {
                   this.$swal({
                       title: "Batal",
                       text : 'Data outlet tidak jadi dihapus',
                       icon: 'error',
                       confirmButtonText: "OK"
                   })
               }
           })
      },
    }
}
</script>
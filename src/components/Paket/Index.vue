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
                  <h2 class="card-title">Paket's data</h2>
                   
                  <div class="table-responsive">
                    <table class="table">
                       <thead>
                                                 
                          <th>#</th>
                          <th>Jenis</th>
                          <th>Harga</th>
                          <th>Aksi</th>
                      
                        </thead>
                      <tbody>

                    <tr v-for="(p, index) in paket" :key="index">
                            <td>{{ index + 1 }}</td>
                            <td>{{ p.jenis }}</td>
                            <td>{{ "Rp " + p.harga }}</td>
                            <td>
                              <button type="button" class="btn btn-warning btn-rounded btn-icon">
                                <router-link :to="{name : 'editpaket' ,params :{id :  p.id_paket }}">
                              <i class="mdi mdi-border-color"></i>
                              </router-link>
                              </button>
                                  <button type="button" @click="hapus( p.id_paket)" class="btn btn-danger btn-rounded btn-icon">
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
            paket :{}
        }
    },
    created(){
        var data = JSON.parse(this.$store.state.datauser)
        var level = data.level
        if(level == 'kasir' || level == 'owner')
        {
            this.$swal("Anda tidak dapat mengakses halaman ini")
            this.$router.push('/') 
        }

        this.axios.get('http://127.0.0.1:8000/api/paket',{
            headers : { Authorization : 'Bearer' + this.$store.state.token}
        })
        .then(res => {
            this.paket = res.data
        })
    },
    methods : {
       hapus(id_paket) {
           this.$swal({
               title: "Hapus Data Paket",
               text: "Apakah anda yakin menghapus data Paket ini?",
               icon: 'waning',
               showDenyButton: true,
               //showCancelButton: "false",
               confirmButtonText: "Ya",
               denyButtonText: "Tidak",
           }).then((result) => {
               console.log(result)
               if(result.isConfirmed){
                 this.axios
                        .delete(`http://127.0.0.1:8000/api/paket/${id_paket}`, {
                        headers: { Authorization: "Bearer " + this.$store.state.token },
                        })
                        .then((res) => {
                        if(res.data.success) {
                        this.$swal("Sukses", res.data.message, "success")
                        let i = this.paket.map(item => item.id_paket).indexOf(id_paket);
        this.paket.splice(i, 1)
                        this.getData()
                        } 
                    })
               } else {
                   this.$swal({
                       title: "Batal",
                       text : 'Data Paket Tidak jadi dihapus',
                       icon: 'error',
                       confirmButtonText: "OK"
                   })
               }
           })
      },
        
    }
}
</script>
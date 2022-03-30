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
                  <h2 class="card-title">User's data</h2>
                  <div class="form-group">
										<div class="input-group">
											<input type="text" class="form-control" name="search" v-model="search" placeholder="Search Here" aria-label="Recipient's username" aria-describedby="basic-addon2"/>
											<div class="input-group-append">
												<button class="btn btn-sm btn-primary" v-on:click="cari_data()" type="button">Search</button>
											</div>
										</div>
									</div>
                   <router-link to="/user/tambah"  class="d-flex flex-row-reverse">
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
                                                <th>#</th>
                                                <th>Nama</th>
                                                <th>Username</th>
                                                <th>level</th>
                                                <th>Outlet</th>
                                                <th>Aksi</th>
                                            </thead>
                      <tbody>
                        
                                            <tr v-for="(u, index) in user" :key="index">
                                                    <td>{{ index + 1 }}</td>
                                                    <td>{{ u.name }}</td>
                                                    <td>{{ u.username }}</td>
                                                    <td>{{ u.level }}</td>
                                                    <td>{{ u.nama_outlet }}</td>
                                                    <td>
                                                      <button type="button" class="btn btn-warning btn-rounded btn-icon">
                                <router-link :to="{name : 'edituser' ,params :{id : u.id}}">
                              <i class="mdi mdi-border-color"></i>
                              </router-link>
                              </button>
                                                        
                                                       <button type="button" @click="hapus(u.id)" class="btn btn-danger btn-rounded btn-icon"><i class="mdi mdi-delete"></i></button>
                                                   
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
            user : {}
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
        
        this.axios.get('http://127.0.0.1:8000/api/user', { headers : { 'Authorization' : 'Bearer ' + this.$store.state.token} })
                  .then(res => {
                      this.user = res.data
                  })
                  .catch(err => console.log(err))
    },
    methods : {
      hapus(id) {
           this.$swal({
               title: "Hapus Data User",
               text: "Apakah anda yakin menghapus data User ini?",
               icon: 'waning',
               showDenyButton: true,
               //showCancelButton: "false",
               confirmButtonText: "Ya",
               denyButtonText: "Tidak",
           }).then((result) => {
               console.log(result)
               if(result.isConfirmed){
                 this.axios
                        .delete(`http://127.0.0.1:8000/api/user/${id}`, {
                        headers: { Authorization: "Bearer " + this.$store.state.token },
                        })
                        .then((res) => {
                        if(res.data.success) {
                        this.$swal("Sukses", res.data.message, "success")
                        let i = this.user.map(item => item.id).indexOf(id);
        this.user.splice(i, 1)
                        this.getData()
                        }
                    })
               } else {
                   this.$swal({
                       title: "Batal",
                       text : 'Data User tidak jadi dihapus',
                       icon: 'error',
                       confirmButtonText: "OK"
                   })
               }
           })
      },
      dt_user: function(){
          this.axios.get("http://127.0.0.1:8000/api/get_user",{
            headers: {Authorization: `Bearer` + this.$store.state.token},
          })
          .then((res)=>{
            this.user = res.data
          })
        },
        cari_data: function(){
          var data_cari ="";
          if(this.search ==""){
            data_cari ="";
          }else{
            data_cari = "/"+ this.search;
          }
          this.axios.get("http://127.0.0.1:8000/api/get_user" + data_cari,{
            headers:	{Authorization:`Bearer` + this.$store.state.token}
          })
          .then((res)=>{
            this.user = res.data
          })
        },
      },
      mounted(){
		this.dt_user();
    }
}
</script>
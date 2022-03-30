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
                  <h2 class="card-title">Transaksi's data</h2>
                  <div class="form-group">
										<div class="input-group">
											<input type="text" class="form-control" name="search" v-model="search" placeholder="Search Here" aria-label="Recipient's username" aria-describedby="basic-addon2"/>
											<div class="input-group-append">
												<button class="btn btn-sm btn-primary" v-on:click="cari_data()" type="button">Search</button>
											</div>
										</div>
									</div>
                   <router-link to="/transaksi/tambah"  class="d-flex flex-row-reverse">
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
                              <th>#</th>
                              <th>Nama Member</th>
                              <th>Tgl Order</th>
                              <!-- <th>Batas Waktu</th> -->
                              <th>Status</th>
                               <th>Petugas</th>
                              <th>Aksi</th>
                              
                          </tr>
                      </thead>
                      <tbody>
                        
                      <tr v-for="(t, index) in transaksi" :key="index">
                              <td>{{ index + 1 }}</td>
                              <td>{{ t.nama_member }}</td>
                              <td>{{ t.tanggal }}</td>
                              <!-- <td>{{ t.batas_waktu }}</td> -->
                              <td>
                                  <span v-if="t.status == 'baru'" class="badge bg-info text-light">{{ t.status }}</span>
                                  <span v-if="t.status == 'proses'" class="badge bg-warning text-dark">{{ t.status }}</span>                                                       
                                  <span v-if="t.status == 'selesai'" class="badge bg-success text-light">{{ t.status }}</span>                                                       
                                  <span v-if="t.status == 'diambil'" class="badge bg-primary text-light">{{ t.status }}</span>
                              </td>
                               <td>{{ t.name }}</td>
                              <td>
                                <button type="button" class="btn btn-secondary btn-rounded btn-icon">
                                <router-link :to="{name : 'detailtransaksi' ,params :{id : t.id_transaksi}}">
                              <i class="mdi mdi-eye"></i>
                              </router-link>
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
    data(){
        return {
            transaksi : {}
        }
    },
    created(){
        var data = JSON.parse(this.$store.state.datauser)
        var level = data.level
        if(level == 'owner')
        {
            this.$swal("Anda tidak dapat mengakses halaman ini")
            this.$router.push('/') 
        }
        
        this.axios.get('http://127.0.0.1:8000/api/transaksi', {headers : { Authorization : 'Bearer' + this.$store.state.token}})
        .then(res =>{
            this.transaksi = res.data.data
        })
        .catch(err => console.log(err))

    },
    methods : {
    dt_transaksi: function(){
          this.axios.get("http://127.0.0.1:8000/api/get_transaksi",{
            headers: {Authorization: `Bearer` + this.$store.state.token},
          })
          .then((res)=>{
            this.transaksi = res.data
          })
        },
        cari_data: function(){
          var data_cari ="";
          if(this.search ==""){
            data_cari ="";
          }else{
            data_cari = "/"+ this.search;
          }
          this.axios.get("http://127.0.0.1:8000/api/get_transaksi" + data_cari,{
            headers:	{Authorization:`Bearer` + this.$store.state.token}
          })
          .then((res)=>{
            this.transaksi = res.data
          })
        },
      },
      mounted(){
		this.dt_transaksi();
	},
}
</script>
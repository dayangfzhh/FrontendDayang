<template>
  <div class="container-scroller">
    <div class="container-fluid page-body-wrapper">
      <sidebar-component></sidebar-component>
      <navbar-component></navbar-component>
      <div class="main-panel">
        <div class="col-lg-12">
      
        <div class="content-wrapper">
          <div class="row">
            <div class="col-12 grid-margin stretch-card">
              <div class="card">
                <div class="card-body">
                  <h4 class="card-title">Add Outlet's Data</h4>

                  <form @submit.prevent="tambah">
                        <div class="form-group">
                            <label>Nama Outlet</label>
                            <input type="text" class="form-control" v-model="outlet.nama_outlet">
                        </div>
                        <div class="form-group">
                            <label>Alamat</label>
                            <textarea rows="4" class="form-control" v-model="outlet.alamat"></textarea>
                        </div>
                        <button type="submit" class="btn btn-success btn-block">Simpan</button>
                    </form>
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
    },
    methods : {
        tambah() {
            this.axios.post('http://127.0.0.1:8000/api/outlet', this.outlet, { headers : { 'Authorization' : `Bearer ` + this.$store.state.token} })
                      .then( (res) => {
                          if(res.data.success) {
                              this.$swal("Sukses", res.data.message, "success")
                              this.$router.push('/outlet');
                          }
                          
                      })
                      .catch( err => console.log(err))
        }
    } 
}
</script>
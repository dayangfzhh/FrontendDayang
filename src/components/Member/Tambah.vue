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
                  <h4 class="card-title">Add Member's Data</h4>

                  <form @submit.prevent="tambah">
                  
                    <div class="form-group">
                      <label>Nama Member</label>
                      <input type="text" class="form-control" v-model="member.nama_member" placeholder="Masukkan Nama">
                    </div>
                    <div class="form-group">
                      <label>Alamat</label>
                      <textarea class="form-control" v-model="member.alamat_member" rows="4" placeholder="Masukkan Alamat"></textarea>
                    </div>
                    <div class="form-group">
                      <label>Jenis Kelamin</label>
                        <select class="form-control" v-model="member.jenis_kelamin">
                          <option value="L">Male</option>
                          <option value="P">Female</option>
                        </select>
                      </div>
                    
                    <div class="form-group">
                      <label>No. Telp</label>
                      <input type="text" class="form-control" v-model="member.no_telp" placeholder="Masukkan Nomor Telpon">
                    </div>
                    
                    <button type="submit" class="btn btn-primary mr-2">Submit</button>
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
            member : {}
        }
    },
    created(){
        var data = JSON.parse(this.$store.state.datauser)
        var level = data.level

        if(level == 'owner')
        {
            this.$swal("Error", "Anda tidak dapat mengakses halaman ini", "error")
            this.$router.push('/')
        }
    },
    methods : {
        tambah() {
            this.axios.post('http://127.0.0.1:8000/api/member', this.member, { headers : { 'Authorization' : `Bearer ` + this.$store.state.token} })
                      .then( (res) => {
                          if(res.data.success) {
                              this.$swal("Sukses", res.data.message, "success")
                              this.$router.push('/member');
                          }
                          
                      })
                      .catch( err => console.log(err))
        }
    } 
}
</script>          
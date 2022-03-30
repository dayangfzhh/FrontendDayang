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
                  <h4 class="card-title">Edit Member's Data</h4>

                  <form @submit.prevent="edit">
                  
                    <div class="form-group">
                      <label>Nama Member</label>
                      <input type="text" class="form-control" v-model="member.nama_member" >
                    </div>
                    <div class="form-group">
                      <label>Alamat</label>
                      <textarea class="form-control" v-model="member.alamat_member" rows="4" ></textarea>
                    </div>
                    <div class="form-group">
                                            <div>
                                                <label>Jenis Kelamin</label>
                                            </div>
                                            <div class="btn-group btn-group-toggle" data-toggle="buttons">                                                
                                                <label v-if="member.jenis_kelamin =='L'" class="btn btn-secondary active">
                                                    <input type="radio" value="L" v-model="member.jenis_kelamin" checked> Laki-laki
                                                </label>
                                                <label v-else class="btn btn-secondary">
                                                    <input type="radio" value="L" v-model="member.jenis_kelamin"> Laki-laki
                                                </label>
                                                <label v-if="member.jenis_kelamin == 'P'" class="btn btn-secondary active">
                                                    <input type="radio" value="P" v-model="member.jenis_kelamin" checked> Perempuan
                                                </label>
                                                <label v-else class="btn btn-secondary">
                                                    <input type="radio" value="P" v-model="member.jenis_kelamin"> Perempuan
                                                </label>
                                            </div>
                                        </div>
                    
                    <div class="form-group">
                      <label>No. Telp</label>
                      <input type="text" class="form-control" v-model="member.no_telp" >
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
    created() {
        var data = JSON.parse(this.$store.state.datauser)
        var level = data.level

        if(level == 'owner')
        {
            this.$swal("Error", "Anda tidak dapat mengakses halaman ini", "error")
            this.$router.push('/')
        }

        this.axios.get(`http://127.0.0.1:8000/api/member/${this.$route.params.id}`, { headers : { 'Authorization' : `Bearer ` + this.$store.state.token} })
             .then((res) => {
                 this.member = res.data
             })
    },
    methods : {
        edit() {
            this.axios.put(`http://127.0.0.1:8000/api/member/${this.$route.params.id}`, this.member, { headers : { 'Authorization' : `Bearer ` + this.$store.state.token} })
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
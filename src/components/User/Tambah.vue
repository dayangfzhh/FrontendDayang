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
                  <h4 class="card-title">Add User's Data</h4>

                  <form @submit.prevent="tambah">
                  
                                        <div class="form-group">
                                            <label>Nama</label>
                                            <input type="text" class="form-control" v-model="user.name" placeholder="Masukkan Nama">
                                        </div>
                                        <div class="form-group">
                                            <label>Username</label>
                                            <input type="text" class="form-control" v-model="user.username" placeholder="Masukkan Username" >
                                        </div>                                        
                                        <div class="form-group">
                                            <label>Password</label>
                                            <input type="password" class="form-control" v-model="user.password" placeholder="Masukkan Password">                                            
                                        </div>
                                        <div class="form-group">
                                            <label>Level</label>
                                            <select class="form-control" v-model="user.level">
                                                <option value="admin">Admin</option>
                                                <option value="kasir">Kasir</option>
                                                <option value="owner">Owner</option>
                                            </select>                                            
                                        </div>
                                        <div class="form-group">
                                            <label>Outlet</label>
                                            <select class="form-control" v-model="user.id_outlet">
                                                <option v-for="(o, index) in outlet" :key="index" :value="o.id_outlet">{{ o.nama_outlet }}</option>
                                            </select>                                            
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
            user : {},
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
        
        this.axios.get('http://127.0.0.1:8000/api/outlet', { headers : { 'Authorization' : `Bearer ` + this.$store.state.token} })
                  .then(res => {
                      this.outlet = res.data
                  })
                  .catch(err => console.log(err))
    },
    methods : {
        tambah() {
            this.axios.post('http://127.0.0.1:8000/api/user/tambah', this.user, { headers : { 'Authorization' : `Bearer ` + this.$store.state.token} })
                      .then( (res) => {
                          if(res.data.success) {
                              this.$swal("Sukses", res.data.message, "success")
                             this.$router.push('/user');
                          }
                      })
                      .catch( err => console.log(err))
        }
    } 
}
</script>   
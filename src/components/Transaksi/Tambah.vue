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
                  <h4 class="card-title">Add Transaksi's Data</h4>

                  <form @submit.prevent="tambah">
                  <div class="form-group">
                        <label>Member</label>
                        <select class="form-control" v-model="transaksi.id_member">
                            <option v-for="(m, index) in member" :key="index" :value="m.id_member">
                                {{m.nama_member}}
                            </option>
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
    data(){
        return{
            member : {},
            transaksi :{}
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
        
        this.axios.get('http://127.0.0.1:8000/api/member',
         { headers : { 'Authorization' : `Bearer ` + this.$store.state.token} })
                      .then( (res) => {
                          this.member = res.data;
                      })
                      .catch( err => console.log(err))
    },
    methods : {
        tambah() {
            this.axios.post('http://127.0.0.1:8000/api/transaksi', this.transaksi, { headers : { 'Authorization' : `Bearer ` + this.$store.state.token} })
                      .then( (res) => {
                          if(res.data.success) {
                              this.$swal("Sukses", res.data.message, "success")
                              this.$router.push('/transaksi');
                          }
                      })
                      .catch( err => console.log(err))
        }
    } 
        
}
</script>
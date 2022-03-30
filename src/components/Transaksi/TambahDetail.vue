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
                  <h4 class="card-title">Add Detail Transaksi</h4>

                  <form @submit.prevent="tambah">
                      <div class="form-group">
                          <label>Jenis Paket</label>
                          <select class="form-control" v-model="detail.id_paket">
                              <option v-for="(p, index) in paket" :key="index" :value="p.id_paket">{{ p.jenis }}</option>
                          </select>
                      </div>
                      <div class="form-group">
                          <label>Jumlah (kg / satuan)</label>
                          <input type="text" class="form-control" v-model="detail.qty">
                      </div>
                      <input type="hidden" v-model="detail.id_transaksi">
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
            id_transaksi : this.$route.params.id,
            paket : {},
            detail : {}
        }
    },
    created() {
        var data = JSON.parse(this.$store.state.datauser)
        var level = data.level
        if(level == 'owner')
        {
            this.$swal("Anda tidak dapat mengakses halaman ini")
            this.$router.push('/') 
        }
        
        this.axios.get('http://127.0.0.1:8000/api/paket', { headers : { 'Authorization' : `Bearer ` + this.$store.state.token} })
             .then( (res) => {
                    this.paket = res.data
                    this.detail.id_transaksi = this.id_transaksi
             })
             .catch(err => console.log(err))
    },    
    methods : {
        tambah() {
            this.axios.post('http://127.0.0.1:8000/api/transaksi/detail/tambah', this.detail, { headers : { 'Authorization' : `Bearer ` + this.$store.state.token} })
                      .then( () => {
                          this.$router.push({ name : 'detailtransaksi', params : this.id_transaksi});
                      })
                      .catch( err => console.log(err))
        }
    } 
}
</script>
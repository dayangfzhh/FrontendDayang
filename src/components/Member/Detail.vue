<template>
  <div class="container-scroller">
    <div class="container-fluid page-body-wrapper">
      <sidebar-component></sidebar-component>
      <navbar-component></navbar-component>
      <div class="main-panel">
        <div class="col-lg-12">
      
        <div class="content-wrapper">
          <div class="container-fluid">
                    <div class="row">
            <div class="col-md-6 grid-margin stretch-card">
              <div class="card">
                <div class="card-body">
                  <p class="card-title">Member Details</p>
                  <div class="d-flex flex-wrap mb-5">
                    <div class="mr-5 mt-3">
                      <p class="text-muted">Nama Member</p>
                      <h3 class="text-primary fs-30 font-weight-medium">{{ member.nama_member }}</h3>
                    </div>
                    <div class="mr-5 mt-3">
                      <p class="text-muted">Alamat Member</p>
                      <h3 class="text-primary fs-30 font-weight-medium">{{ member.alamat_member }}</h3>
                    </div>
                    <div class="mr-5 mt-3">
                      <p class="text-muted">Jenis Kelamin</p>
                      <h3 class="text-primary fs-30 font-weight-medium">{{ member.jenis_kelamin }}</h3>
                    </div>
                    <div class="mt-3">
                      <p class="text-muted">No.Telp</p>
                      <h3 class="text-primary fs-30 font-weight-medium">{{ member.no_telp }}</h3>
                    </div> 
                  </div>
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
            id : this.$route.params.id,
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

        this.axios.get(`http://127.0.0.1:8000/api/member/${this.id}`, { headers : { 'Authorization' : 'Bearer ' + this.$store.state.token} })
             .then(res => {
                 this.member = res.data
             })
    }
}
</script>
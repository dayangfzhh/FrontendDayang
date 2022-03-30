<template>
  <div class="container-scroller">
    <div class="container-fluid page-body-wrapper">
      <sidebar-component></sidebar-component>
      <navbar-component></navbar-component>
      <div class="main-panel">
        <div class="content-wrapper">
          <div class="row">
            <div class="col-md-12 grid-margin">
              <div class="row">
                <div class="col-12 col-xl-8 mb-4 mb-xl-0">
                  <h3 class="font-weight-bold">Welcome {{ name }}, {{ level }}</h3>
                  <h6 class="font-weight-normal mb-0">
                    All systems are running smoothly!
                  </h6>
                </div>
              </div>
            </div>
          </div>
          <div class="row">
            <div class="col-md-6 grid-margin stretch-card">
              <div class="card">
                <div class="card mt-auto">
                  <img src="images/dashboard/laund-dash.png" alt="" />
                    </div>
                  
              </div>
            </div>
            <div class="col-md-6 grid-margin transparent">
              <div class="row">
                <div class="col-md-6 mb-4 stretch-card transparent">
                  <div class="card card-tale">
                    <div class="card-body">
                      <p class="mb-4">Member Person</p>
                      <p class="fs-30 mb-2">{{ data.member }}</p>
                     
                    </div>
                  </div>
                </div>
                <div class="col-md-6 mb-4 stretch-card transparent">
                  <div class="card card-dark-blue">
                    <div class="card-body">
                      <p class="mb-4">New Order's</p>
                      <p class="fs-30 mb-2">{{ data.baru }}</p>
                      
                    </div>
                  </div>
                </div>
              </div>
              <div class="row">
                <div class="col-md-6 mb-4 mb-lg-0 stretch-card transparent">
                  <div class="card card-light-blue">
                    <div class="card-body">
                      <p class="mb-4">Laundry Process</p>
                      <p class="fs-30 mb-2">{{ data.proses }}</p>
                      
                    </div>
                  </div>
                </div>
                <div class="col-md-6 stretch-card transparent">
                  <div class="card card-light-danger">
                    <div class="card-body">
                      <p class="mb-4">Your Income</p>
                      <p class="fs-30 mb-2">Rp {{ data.pendapatan }}</p>
                     
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <!-- content-wrapper ends -->
        <footer-component></footer-component>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
        return {
            name : '',
            level : '',
            data : {}            
        }
    },
    created() {
        var data = JSON.parse(this.$store.state.datauser)
        this.name = data.name
        this.level = data.level
        this.axios.get('http://127.0.0.1:8000/api/dashboard', { headers : { 'Authorization' : 'Bearer ' + this.$store.state.token} })
             .then(res => {
                 this.data = res.data
             })
    }
}
</script>
<template>
    <div class="container-scroller">
    <div class="container-fluid page-body-wrapper full-page-wrapper">
      <div class="content-wrapper d-flex align-items-center auth px-0">
        <div class="row w-100 mx-0">
          <div class="col-lg-4 mx-auto">
            <div class="auth-form-light text-left py-5 px-4 px-sm-5">
              <div class="brand-logo">
                <img src="../../public/images/retalaundryy.png" alt="logo">
              </div>
              <h4>Hello! let's get started</h4>
              <h6 class="font-weight-light">Sign in to continue.</h6>
              <form class="pt-3">
            <form class="user" @submit.prevent="login">
                <div class="form-group">
                  <input type="text" class="form-control form-control-lg" placeholder="Masukkan Username" v-model="account.username">
                </div>
                <div class="form-group">
                  <input type="password" class="form-control form-control-lg" placeholder="Masukkan Password" v-model="account.password">
                </div>
                  <button type="submit" class="btn btn-block btn-primary btn-lg font-weight-medium auth-form-btn">
                      SIGN IN
                </button>
                </form>
              </form>
            </div>
          </div>
        </div>
      </div>
      <!-- content-wrapper ends -->
    </div>
    <!-- page-body-wrapper ends -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      account: {},
    };
  },
  methods: {
    login() {
      this.axios
        .post("http://127.0.0.1:8000/api/login", this.account)
        .then((res) => {
          if (res.data.success) {
            this.$store.commit("setToken", res.data.token);
            this.$store.commit("setUser", JSON.stringify(res.data.user));
            this.$store.commit('setOutlet', JSON.stringify(res.data.outlet)) 
            this.$router.push("/");
          }
        })
        .catch((err) => console.log(err));
    },
  },
};
</script>
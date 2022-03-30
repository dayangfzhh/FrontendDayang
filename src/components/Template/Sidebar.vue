<template>
  <nav class="sidebar sidebar-offcanvas" id="sidebar">
    <ul class="nav">
      <li class="nav-item">
        <router-link to="/" class="nav-link" href="index.html">
          <i class="mdi mdi-home menu-icon"></i>
          <span class="menu-title">Dashboard</span>
        </router-link>
      </li>
      
      <li v-if="isAdmin" class="nav-item">
        <router-link to="/user" class="nav-link" href="index.html">
          <i class="mdi mdi-account menu-icon"></i>
          <span class="menu-title">Users</span>
        </router-link>
      </li>
      <li  v-if="isAdmin || isKasir" class="nav-item">
        <router-link to="/member" class="nav-link" href="index.html">
         <i class="mdi mdi-account-multiple menu-icon"></i>
          <span class="menu-title">Member</span>
        </router-link>
      </li>
      <li v-if="isAdmin" class="nav-item">
        <router-link to="/outlet" class="nav-link" href="index.html">
          <i class="mdi mdi-store menu-icon"></i>
          <span class="menu-title">Outlet</span>
        </router-link>
      </li>
      <li  v-if="isAdmin" class="nav-item">
        <router-link to="/paket" class="nav-link" href="index.html">
          <i class="mdi mdi-package-variant menu-icon"></i>
          <span class="menu-title">Data Paket</span>
        </router-link>
      </li>
      <li v-if="isAdmin || isKasir" class="nav-item">
        <router-link to="/transaksi" class="nav-link" href="index.html">
          <i class="mdi mdi-file menu-icon"></i>
          <span class="menu-title">Transaksi</span>
        </router-link>
      </li>
      <li class="nav-item">
        <router-link to="/report" class="nav-link" href="index.html">
          <i class="mdi mdi-folder menu-icon"></i>
          <span class="menu-title">Report</span>
        </router-link>
      </li>
    </ul>
  </nav>
</template>

<script>
export default {
    data(){
        return{
            isAdmin : false,
            isKasir : false,
            isOwner : false,
        }
    },
    created() {
        var user = JSON.parse(this.$store.state.datauser)
        var level = user.level

        if(level == 'admin')this.isAdmin = true
        else if(level == 'kasir')this.isKasir =true
        else this.isOwner = true
    }
    
}
</script>
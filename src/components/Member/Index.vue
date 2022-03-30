<template>
<div class="container-scroller">
	<div class="container-fluid page-body-wrapper">
		<sidebar-component></sidebar-component>
		<navbar-component></navbar-component>
		<div class="main-panel">
			<div class="col-lg-12">
				<div class="content-wrapper">
					<div class="row">
						<div class="col-lg-12 grid-margin stretch-card">
							<div class="card">
								<div class="card-body">
									<h2 class="card-title">Member's data</h2>
									<div class="form-group">
										<div class="input-group">
											<input type="text" class="form-control" name="search" v-model="search" placeholder="Search Here" aria-label="Recipient's username" aria-describedby="basic-addon2"/>
											<div class="input-group-append">
												<button class="btn btn-sm btn-primary" v-on:click="cari_data()" type="button">Search</button>
											</div>
										</div>
									</div>

									<div class="form-group"></div>
									<router-link to="/member/tambah" class="d-flex flex-row-reverse">
									<div class="btn btn-primary btn-icon-split btn-rounded">
										<div class="mx-1">
											<span class="icon text-white-50">
											<i class="mdi mdi-plus"></i>
											</span>
											Add
										</div>
									</div>
									</router-link>
									<div class="table-responsive">
										<table class="table">
										<thead>
										<tr>
											<th>No.</th>
											<th>Nama</th>
											<th>Alamat</th>
											<th>Aksi</th>
										</tr>
										</thead>
										<tbody>
										<tr v-for="(m, index) in member" :key="index">
											<td>{{index+1}}</td>
											<td>{{m.nama_member}}</td>
											<td>{{m.alamat_member}}</td>
											<td>
												<button type="button" class="btn btn-secondary btn-rounded btn-icon">
													<router-link :to="{name : 'detailmember' ,params :{id : m.id_member}}">
												<i class="mdi mdi-eye"></i>
												</router-link>
												</button>
												<button type="button" class="btn btn-warning btn-rounded btn-icon">
													<router-link :to="{name : 'editmember' ,params :{id : m.id_member}}">
												<i class="mdi mdi-border-color"></i>
												</router-link>
												</button>
												
												<button type="button" @click="hapus(m.id_member)" class="btn btn-danger btn-rounded btn-icon">
												<i class="mdi mdi-delete"></i>
												</button>
												
											</td>
										</tr>
										</tbody>
										</table>
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
            member :{}
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
        this.axios.get('http://127.0.0.1:8000/api/member',{
            headers : { Authorization : 'Bearer' + this.$store.state.token}
        })
        .then(res => {
            this.member = res.data
        })
    },
    methods :{
		dt_member: function(){
			this.axios.get("http://127.0.0.1:8000/api/get_member",{
				headers: {Authorization: `Bearer` + this.$store.state.token},
			})
			.then((res)=>{
				this.member = res.data
			})
		},
		cari_data: function(){
			var data_cari ="";
			if(this.search ==""){
				data_cari ="";
			}else{
				data_cari = "/"+ this.search;
			}
			this.axios.get("http://127.0.0.1:8000/api/get_member" + data_cari,{
				headers:	{Authorization:`Bearer` + this.$store.state.token}
			})
			.then((res)=>{
				this.member = res.data
			})
		},
		hapus(id_member) {
           this.$swal({
               title: "Hapus Data Member",
               text: "Apakah anda yakin menghapus data member ini?",
               icon: 'waning',
               showDenyButton: true,
               //showCancelButton: "false",
               confirmButtonText: "Ya",
               denyButtonText: "Tidak",
           }).then((result) => {
               console.log(result)
               if(result.isConfirmed){
                 this.axios
                        .delete(`http://127.0.0.1:8000/api/member/${id_member}`, {
                        headers: { Authorization: "Bearer " + this.$store.state.token },
                        })
                        .then((res) => {
                        if(res.data.success) {
                        this.$swal("Sukses", res.data.message, "success")
                        let i = this.member.map(item => item.id_member).indexOf(id_member);
        this.member.splice(i, 1)
                        this.getData()
                        }
                    })
               } else {
                   this.$swal({
                       title: "Batal",
                       text : 'Data Member tidak jadi dihapus',
                       icon: 'error',
                       confirmButtonText: "OK"
                   })
               }
           })
		},
        // hapus(id_member){
			
        //     this.axios.delete(`http://127.0.0.1:8000/api/member/${id_member}`,{
        //         headers : { Authorization : 'Bearer' + this.$store.state.token}
        //     })
        //     .then( (res) => {
        //                   if(res.data.success) {
        //                       let i = this.member.map(item => item.id_member).indexOf(id_member);
        //                       this.member.splice(i, 1)
        //                       this.$swal(res.data.message) 
        //                   } 
        //               })
        //               .catch(err => console.log(err))
        // },
    },
	mounted(){
		this.dt_member();
	},
}
</script>
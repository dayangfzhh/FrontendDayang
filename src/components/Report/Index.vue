<template>
  <div class="container-scroller">
    <div class="container-fluid page-body-wrapper">
      <sidebar-component></sidebar-component>
      <navbar-component></navbar-component>
      <div class="main-panel">
        <div class="col-lg-12">
      
        <div class="content-wrapper">
          <div class="row">
            <div class="col-lg-12 ">
              <div class="card">
                <div class="card-body">
                  <h2 class="card-title">Cetak Report Transaksi</h2>
                 
                    <form class="form-group" @submit.prevent="tampil">
                        <form class="form-group" @submit.prevent="tampil">
                        <label>Bulan</label>
                        <select class="form-control" v-model="report.bulan">
                            <option value="01">Januari</option>
                            <option value="02">Februari</option>
                            <option value="03">Maret</option>
                            <option value="04">April</option>
                            <option value="05">Mei</option>
                            <option value="06">Juni</option>
                            <option value="07">Juli</option>
                            <option value="08">Agustus</option>
                            <option value="09">September</option>
                            <option value="10">Oktober</option>
                            <option value="11">November</option>
                            <option value="12">Desember</option>
                        </select>
                        <br />
                        <label>Tahun</label>
                        <select class="form-control" v-model="report.tahun">
                            <option value="2020">2020</option>
                            <option value="2021">2021</option>
                            <option value="2022">2022</option>
                        </select>
                        <button type="submit" class="btn btn-sm btn-secondary">
                            Tampilkan
                        </button>
                        </form>
                    </form>
                    <div class="report">
                        <VueHtml2pdf
                        :show-layout="true"
                        :float-layout="false"
                        :enable-download="true"
                        :preview-modal="true"
                        :paginate-elements-by-height="1400"
                        filename="Report_transaksi"
                        :pdf-quality="2"
                        :manual-pagination="false"
                        pdf-format="a4"
                        pdf-orientation="portrait"
                        pdf-content-width="800px"
                        ref="html2Pdf"
                        >
                        <section slot="pdf-content">
                            <h1 class="text-center">Report Transaksi</h1>
                            <h3 class="text-center">Laundry Online</h3>
                            <h3>{{ outlet.nama_outlet }}</h3>
                                        <h5>{{ outlet.alamat }}</h5>
                            <br /><br />
                            <table class="table table-bordered text-center">
                            <thead>
                                <tr>
                                <th class="font-weight-bold">No</th>
                                <th class="font-weight-bold">Member</th>
                                <th class="font-weight-bold">Tanggal Transaksi</th>
                                <th class="font-weight-bold">Tanggal Pembayaran</th>
                                <th class="font-weight-bold">Nominal Pembayaran</th>
                                <th class="font-weight-bold">Petugas</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(t, index) in transaksi" :key="index">
                                <td>{{ index + 1 }}</td>
                                <td>{{ t.nama_member }}</td>
                                <td>{{ t.tanggal | moment("DD/MM/YYYY") }}</td>
                                <td v-if="t.tanggal_bayar == null">Belum Dibayar</td>
                                <td v-else>
                                    {{ t.tanggal_bayar | moment("DD/MM/YYYY") }}
                                </td>
                                <td v-if="t.total == null">Rp. </td>
                                <td v-else>Rp {{ t.total }}</td>
                                 <td>{{ t.name }}</td>
                                </tr>
                            </tbody>
                            </table>
                        </section>
                        </VueHtml2pdf>
                        <button type="button" class="btn btn-sm btn-secondary" @click="generateReport">
                        Buat Report
                        </button>
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

<style scoped>
h1,
h3,
h5 {
  text-align: center;
}
.report {
  width: 800px;
  margin: 20px auto 0 auto;
}
table {
  margin: 20px;
  border-collapse: collapse;
  width: 700px;
}
thead tr {
  background-color: #5465ff;
  color: white;
  text-align: left;
}
th,
td {
  padding: 5px;
}
tbody tr {
  border-bottom: 1px solid #eee;
}
tbody tr:nth-of-type(even) {
  background-color: #f3f3f3;
}
tbody tr:last-of-type {
  border-bottom: 2px solid #0010a3;
}
button {
  width: 120px;
  height: 35px;
  margin-top: 20px;
  margin-left: 20px;
  border: none;
  background-color: #788bff;
  color: white;
  border-radius: 3px;
}
button:hover {
  background-color: #335fff;
}
button:active {
  background-color: #0028b8;
}
</style>
<script>
export default {
  data() {
    return {
      report: {},
      transaksi: {},
      outlet: {}
    };
  },
  created() {
        var dataa = JSON.parse(this.$store.state.dataoutlet)
        this.outlet = dataa
        
        var data = JSON.parse(this.$store.state.datauser)
        var id_outlet = data.id_outlet
        
        var date = new Date();
        this.report.id_outlet = id_outlet;
        this.report.tahun = date.getFullYear();
        this.report.bulan = ("0" + (date.getMonth() + 1)).slice(-2);
  },
  methods: {
    tampil() {
      this.axios
        .post(
          "http://127.0.0.1:8000/api/report",
          this.report,
          {
            headers: { Authorization: "Bearer " + this.$store.state.token },
          }
        )
        .then((res) => {
          this.transaksi = res.data;
        })
        .catch((err) => console.log(err));
    },
    generateReport() {
      this.$refs.html2Pdf.generatePdf();
    },
  },
};
</script>
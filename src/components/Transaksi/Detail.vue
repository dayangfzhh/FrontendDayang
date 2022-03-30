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
                <div class="col-md-12 grid-margin stretch-card">
                  <div class="card">
                    <div class="card-body">
                      <p class="card-title">Transaksi Details</p>
                      <div class="d-flex flex-wrap mb-5">
                        <div class="col mr-2">
                          <div class="row">
                            <div class="col">
                              <div class="mr-5 mt-3">
                                <p class="text-muted">Member</p>
                                <h5
                                  class="text-primary fs-30 font-weight-medium"
                                >{{ transaksi.nama_member }}</h5>
                              </div>
                              <div class="mr-5 mt-3">
                                <p class="text-muted">Tanggal Order</p>
                                <h5
                                  class="text-primary fs-30 font-weight-medium"
                                >{{ transaksi.tanggal | moment("DD/MM/YYYY") }}</h5>
                              </div>
                              <div class="mr-5 mt-3">
                                <p class="text-muted">Batas Waktu</p>
                                <h5
                                  class="text-primary fs-30 font-weight-medium"
                                >{{ transaksi.batas_waktu | moment("DD/MM/YYYY") }}</h5>
                              </div>
                            </div>
                            <div class="col">
                              <div class="mr-5 mt-3">
                                <p class="text-muted">Status Laundry</p>
                                <h5
                                  class="text-primary fs-30 font-weight-medium"
                                >{{ transaksi.status }}</h5>
                              </div>
                              <div class="text-muted">Tanggal Pembayaran</div>
                              <div
                                v-if="transaksi.tanggal_bayar == null"
                                class="text-primary fs-30 font-weight-medium"
                              >-</div>
                              <div
                                v-else
                                class="text-primary fs-30 font-weight-medium"
                              >{{ transaksi.tanggal_bayar | moment("DD/MM/YYYY") }}</div>
                              <div class="text-muted">Status Pembayaran</div>
                              <div
                                v-if="transaksi.dibayar == 'belum_dibayar'"
                                class="text-primary fs-30 font-weight-medium"
                              >Belum dibayar</div>
                              <div
                                v-else
                                class="text-primary fs-30 font-weight-medium"
                              >Sudah dibayar</div>
                            </div>
                          </div>
                          <div class="row mt-3">
                            <div class="col">
                              <button
                                :disabled="disableStatus"
                                type="button"
                                class="btn btn-warning"
                                @click="ubahStatus(id_transaksi)"
                              >Ubah Status Laundry</button>
                            </div>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
              <div class="report">
                <VueHtml2pdf
                  :show-layout="true"
                  :float-layout="false"
                  :enable-download="true"
                  :preview-modal="true"
                  :paginate-elements-by-height="1400"
                  filename="Struk Laundry"
                  :pdf-quality="2"
                  :manual-pagination="false"
                  pdf-format="a4"
                  pdf-orientation="portrait"
                  pdf-content-width="900px"
                  ref="html2Pdf"
                >
                  <section slot="pdf-content">
                    <div class="row mt-3">
                      <div class="col">
                        <div class="card shadow mb-4">
                          <br />
                          <center>
                            <div class="brand-logo">
                            <img src="../../../public/images/retalaundrymono.png" width="100" alt="logo">
                          </div>
                          </center>
                          <h2 class="text-center"> Retta Laundry</h2>
                          <h5 class="text-center">{{ alamat }}</h5>
                          <!-- <hr />
                          <h3 class="text-center"> Bukti transaksi</h3>
                          <hr /> -->
                          <br />
                          <h6>&nbsp; &nbsp; &nbsp; Customer : {{ transaksi.nama_member }}</h6>
                          <h6>&nbsp; &nbsp; &nbsp; Order Time : {{ transaksi.tanggal }} </h6>
                          <h6>&nbsp; &nbsp; &nbsp; Cashier : {{ name }} </h6>
                          <div class="card-body">
                            <table class="table">
                              <thead>
                                <tr>
                                  <th>No</th>
                                  <th>Jenis</th>
                                  <th>Jumlah (KG / Satuan)</th>
                                  <th>Subtotal</th>
                                </tr>
                              </thead>
                              <tbody>
                                <tr v-for="(d, index) in detail_transaksi" :key="d.id_transaksi">
                                  <td>{{ index + 1 }}</td>
                                  <td>{{ d.jenis }}</td>
                                  <td>{{ d.qty }}</td>
                                  <td>Rp {{ d.subtotal }}</td>
                                </tr>
                                <tr v-if="total != ''">
                                  <td colspan="3" class="text-right">Total</td>
                                  <td>
                                    <h6 class="font-weight-bold">Rp {{ total }}</h6>
                                  </td>
                                </tr>
                              </tbody>
                            </table>
                          </div>
                        </div>
                      </div>
                    </div>
                  </section>
                </VueHtml2pdf>
                <button
                  :disabled="disableBayar"
                  type="button"
                  class="btn btn-success mr-3"
                  @click="bayar"
                >Bayar</button>
                <router-link
                  v-if="transaksi.status != 'diambil' &&transaksi.status != 'selesai' &&transaksi.dibayar != 'dibayar'"
                  :to="{name: 'tambahdetail',params: { id: this.id_transaksi },}"
                  class="btn btn-primary mr-3"
                >Tambah Detail Transaksi</router-link>
                <button
                  :disabled="disableBayar"
                  type="button"
                  @click="generateReport"
                  class="btn btn-sm btn-info btn-icon-split"
                >
                  <span class="icon text-white-50">
                    <i class="mdi mdi-printer menu-icon"></i>
                  </span>
                  <span class="text">Cetak Struk</span>
                </button>
                <br />
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
.report {
  width: 1000px;
  margin: 20px auto 0 auto;
}
</style>
<script>
export default {
  data() {
    return {
      id_transaksi: this.$route.params.id,
      name : '',
      // id_member : '',
      transaksi: {},
      // users: {},
      detail_transaksi: {},
      total: ""
    };
  },
  created() {
    var dataa = JSON.parse(this.$store.state.dataoutlet)
    var alamat = dataa.alamat
        this.alamat = alamat
    var data = JSON.parse(this.$store.state.datauser);
    this.name = data.name
    var level = data.level;
    if (level == "owner") {
      this.$swal("Anda tidak dapat mengakses halaman ini");
      this.$router.push("/");
    }
    this.axios
      .get(`http://127.0.0.1:8000/api/transaksi/${this.id_transaksi}`, {
        headers: { Authorization: `Bearer ` + this.$store.state.token }
      })
      .then(res => {
        this.transaksi = res.data;
      })
      .catch(err => console.log(err));
    this.axios
      .get(`http://127.0.0.1:8000/api/transaksi/detail/${this.id_transaksi}`, {
        headers: { Authorization: `Bearer ` + this.$store.state.token }
      })
      .then(res => {
        this.detail_transaksi = res.data;
        console.log(res.data);
      })
      .catch(err => console.log(err));
    this.axios
      .get(`http://127.0.0.1:8000/api/transaksi/total/${this.id_transaksi}`, {
        headers: { Authorization: `Bearer ` + this.$store.state.token }
      })
      .then(res => {
        this.total = res.data.total;
      });
  },
  computed: {
      disableStruk() {
    if (
      this.transaksi.status == "baru"
    ) {
      return true;
    } else {
      return false;
    }
  },
    disableBayar() {
      if (
        this.transaksi.status == "baru" ||
        this.transaksi.status == "proses" ||
        this.transaksi.status == "diambil"
      ) {
        return true;
      } else {
        return false;
      }
    },
    disableStatus() {
      if (
        this.transaksi.status == "selesai" ||
        this.transaksi.status == "diambil" ||
        this.detail_transaksi.length == 0
      ) {
        return true;
      } else {
        return false;
      }
    }
  },

  methods: {
    ubahStatus() {
      if (this.transaksi.status == "baru") {
        this.transaksi.status = "proses";
      } else {
        this.transaksi.status = "selesai";
      }
      this.axios
        .post(
          `http://127.0.0.1:8000/api/transaksi/status/${this.id_transaksi}`,
          this.transaksi,
          {
            headers: { Authorization: `Bearer ` + this.$store.state.token }
          }
        )
        .then(() => {
          this.$router.go(1);
        })
        .catch(err => console.log(err));
    },
    bayar() {
      this.axios
        .post(
          `http://127.0.0.1:8000/api/transaksi/bayar/${this.id_transaksi}`,
          this.transaksi,
          {
            headers: { Authorization: `Bearer ` + this.$store.state.token }
          }
        )
        .then(() => {
          this.$router.go();
        })
        .catch(err => console.log(err));
    },
    generateReport() {
      this.$refs.html2Pdf.generatePdf();
    }
  }
};
</script>
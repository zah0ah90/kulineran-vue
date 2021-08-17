<template>
  <div class="keranjang">
    <Navbar :updateKeranjang="keranjangs" />
    <div class="container">
      <!-- breadcrumb -->
      <div class="row mt-5">
        <div class="col">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <router-link to="/" class="text-dark">Home</router-link>
              </li>
              <li class="breadcrumb-item">
                <router-link to="/foods" class="text-dark">Food</router-link>
              </li>
              <li class="breadcrumb-item active" aria-current="page">
                Keranjang
              </li>
            </ol>
          </nav>
        </div>
      </div>
      <!-- breadcrumb -->

      <div class="row mt-3">
        <div class="col">
          <h2>Keranjang <strong>Saya</strong></h2>
          <div class="table-responsive mt-3">
            <table class="table">
              <thead>
                <th>No</th>
                <th>Foto</th>
                <th>Makanan</th>
                <th>Keterangan</th>
                <th>Jumlah</th>
                <th>Harga</th>
                <th>Total Harga</th>
                <th>Hapus</th>
              </thead>
              <tbody>
                <tr
                  v-for="(keranjang, index) in keranjangs"
                  :key="keranjang.id"
                >
                  <td>{{ index + 1 }}</td>
                  <td>
                    <img
                      :src="'assets/images/' + keranjang.products.gambar"
                      class="img-fluid"
                      width="250"
                    />
                  </td>
                  <td>
                    <strong>{{ keranjang.products.nama }}</strong>
                  </td>
                  <td>
                    {{ keranjang.keterangan ? keranjang.keterangan : " - " }}
                  </td>
                  <td>
                    {{ keranjang.jumlah_pesan }}
                  </td>
                  <td align="right">Rp. {{ keranjang.products.harga }}</td>
                  <td align="right">
                    Rp.
                    <strong>{{
                      keranjang.products.harga * keranjang.jumlah_pesan
                    }}</strong>
                  </td>
                  <td align="center" class="text-danger">
                    <b-icon-trash
                      @click="hapusKeranjang(keranjang.id)"
                    ></b-icon-trash>
                  </td>
                </tr>

                <tr>
                  <td colspan="6" align="right">Total :</td>
                  <td>
                    Rp. <strong>{{ totalHarga }}</strong>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>

      <!-- form checkout -->
      <div class="row justify-content-end">
        <div class="col-md-4">
          <form class="mt-4" v-on:submit.prevent>
            <div class="form-group">
              <label for="">Nama :</label>
              <input type="text" class="form-control" v-model="pesan.nama" />
            </div>
            <div class="form-group">
              <label for="">Nomer Meja :</label>
              <input
                type="number"
                class="form-control"
                v-model="pesan.noMeja"
              />
            </div>
            <button @click="checkout" type="submit" class="btn btn-success">
              <b-icon-cart></b-icon-cart>
              Pesan
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Navbar from "../components/Navbar.vue";
export default {
  name: "Keranjang",
  components: {
    Navbar,
  },

  data() {
    return {
      keranjangs: [],
      pesan: {},
    };
  },

  methods: {
    setKeranjang(data) {
      this.keranjangs = data;
    },
    hapusKeranjang(id) {
      axios
        .delete("http://localhost:3000/keranjangs/" + id)
        .then(() => {
          this.$toast.error("Berhasil hapus keranjang", {
            position: "top-right",
            type: "error",
            duration: 3000,
            dismissible: true,
          });

          // refresh table
          axios
            .get("http://localhost:3000/keranjangs")
            .then((response) => this.setKeranjang(response.data))
            .catch((err) => console.log(err));
        })
        .catch((err) => console.log(err));
    },
    checkout() {
      // console.log(this.pesan);
      if (this.pesan.nama && this.pesan.noMeja) {
        this.pesan.keranjangs = this.keranjangs;
        axios
          .post("http://localhost:3000/pesanans", this.pesan)
          .then(() => {
            // delete keranjang
            this.keranjangs.map(function (item) {
              return axios
                .delete("http://localhost:3000/keranjangs/" + item.id)
                .catch((err) => console.log(err));
            });

            this.$router.push({ path: "/pesanan-sukses" });
            this.$toast.success("Sukses Dipesan", {
              position: "top-right",
              type: "success",
              duration: 3000,
              dismissible: true,
            });
          })
          .catch((err) => console.log(err));
      } else {
        this.$toast.error("Mohon untuk mengisi Nama dan No Meja", {
          position: "top-right",
          type: "error",
          duration: 3000,
          dismissible: true,
        });
      }
    },
  },

  mounted() {
    axios
      .get("http://localhost:3000/keranjangs")
      .then((response) => this.setKeranjang(response.data))
      .catch((err) => console.log(err));
  },

  computed: {
    totalHarga() {
      return this.keranjangs.reduce(function (item, data) {
        return item + data.products.harga * data.jumlah_pesan;
      }, 0);
    },
  },
};
</script>

<style>
</style>
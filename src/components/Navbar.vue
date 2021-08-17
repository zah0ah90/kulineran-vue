<template>
  <b-navbar toggleable="lg" type="light">
    <div class="container">
      <!-- <b-navbar-brand href="#">Kulineran</b-navbar-brand> -->

      <router-link class="navbar-brand" to="/">Kulineran</router-link>

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav>
          <router-link class="nav-link" to="/">Home</router-link>
          <router-link class="nav-link" to="/foods">Foods</router-link>
        </b-navbar-nav>

        <!-- Right aligned nav items -->
        <b-navbar-nav class="ml-auto">
          <router-link class="nav-link" to="/keranjang"
            >Keranjang
            <b-icon-bag></b-icon-bag>
            <span class="badge badge-success ml-1">{{
              updateKeranjang ? updateKeranjang.length : jumlah_pesanan.length
            }}</span>
          </router-link>
        </b-navbar-nav>
      </b-collapse>
    </div>
  </b-navbar>
</template>

<script>
import axios from "axios";
export default {
  name: "Navbar",
  data() {
    return {
      jumlah_pesanan: [],
    };
  },

  props: ["updateKeranjang"],

  methods: {
    setJumlah(data) {
      this.jumlah_pesanan = data;
    },
  },

  mounted() {
    axios
      .get("http://localhost:3000/keranjangs")
      .then((response) => {
        this.setJumlah(response.data);
      })
      .catch((err) => console.log(err));
  },
};
</script>

<style>
</style>
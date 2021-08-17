<template>
  <div class="hero">
    <div class="row mt-4">
      <div class="col-sm-6 mb-4">
        <img src="../assets/images/hero.svg" width="100%" alt="" />
      </div>
      <div class="col-sm-6">
        <div class="d-flex h-100">
          <div class="justify-content-center align-self-center">
            <h2><strong>Delicius Food Menu</strong><br />in Your Gadget</h2>
            <p>Ayo segera pesan dan pilih makanan favorit anda</p>
            <router-link to="/foods" class="btn btn-success"
              ><b-icon-arrow-right></b-icon-arrow-right> Pesan</router-link
            >
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Hero",
};
</script>

<style>
</style>
<template>
  <div>
    <input type="checkbox" id="sidebar-toggle" />
    <div class="fr-sidebar">
      <div class="fr-sidebar-header">
        <h3 class="fr-brand">
          <span class="ti-unlink"></span>
          <span class="t-spn">ADMIN SELLER</span>
        </h3>
        <label for="sidebar-toggle" class="ti-menu-alt"></label>
      </div>
      <div class="fr-sidebar-menu">
        <ul>
          <li>
            <a href="/">
              <span class="fr-ti-home"></span>
              <span>Home</span>
            </a>
          </li>
          <li>
            <a href="">
              <span class="fr-ti-face-smile"></span>
              <span>Produk</span>
            </a>
          </li>
          <li>
            <a href="/Dashboard">
              <span class="fr-ti-agenda"></span>
              <span>Dashboard</span>
            </a>
          </li>
          <li>
            <a href="">
              <span class="fr-ti-clipboard"></span>
              <span>Edit Produk</span>
            </a>
          </li>
          <li>
            <a href="">
              <span class="fr-ti-folder"></span>
              <span>Hapus Produk</span>
            </a>
          </li>
          <li>
            <button @click="cek()">
              <span class="fr-ti-time"></span>
              <span>Transaksi</span>
            </button>
          </li>
        </ul>
      </div>
    </div>

    <div class="fr-main-content">
      <header>
        <div class="fr-search-wrapper">
          <span class="ti-search"></span>
          <input type="search" placeholder="Search" />
        </div>
        <div class="fr-social-icons">
          <span class="ti-bell"></span>
          <span class="ti-comment"></span>
          <div></div>
        </div>
      </header>
      <main>
        
      </main>
    </div>
  </div>
</template>

<script>

export default {
   components: {
     App
   },
   data(){
     return{
     
     }
   },
  methods: {
   
  },
  mounted(){

  }
};
</script>
<style scoped>
.btn-create-store{
    box-shadow: 0px 3px 15px 0px #888888;
    background:#28df99;
    text-align:center;
    border: none;
    border-radius: 25px;
    position: absolute;
    width: 140px;
    height: 50px;
    top: 50%;
    left: 50%;
    margin-left: -50px;
    margin-top: -25px;
}
button:focus { outline:0 !important; }
</style>
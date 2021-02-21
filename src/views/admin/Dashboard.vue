<template>
  <div>
    <NavBar/>
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
        <!-- <NewProduk v-show="this.compForm"></NewProduk>
        <Main v-show="this.compMain"></Main>
        <button class="btn-create-store" v-show="this.btnStore">
           <p style="margin:auto; color:white !important;"><span>+ </span><b>Buat Toko</b></p>
        </button> -->
      </main>
    </div>
  </div>
</template>

<script>
//import Main from "../../components/admin/Parent";
import NavBar from "../../components/admin/NavBar1";
import axios from 'axios';
export default {
   components: {
    //  Main,
      NavBar
   },
   data(){
     return{
       compMain:false,
       compForm:false,
       btnStore:false
     }
   },
  methods: {
    cek(){
      var isForm = this.showform
      if(isForm == false){
          this.showform = true
      }else if(isForm == true){
        this.showform = false
      }
    },

    Logout() {
      localStorage.removeItem("user-id");
      localStorage.removeItem("username");
      localStorage.removeItem("email");
      localStorage.removeItem("role_user");
      this.$router.replace({ name: "Home" });
    },
  },
  mounted(){
     var bodyFormData = new FormData();
      bodyFormData.append("user_id",localStorage.getItem("user-id"));
    axios({
        method: "post",
        url:`${this.$api}check_store`,
        data: bodyFormData,
        headers: { "Content-Type": "multipart/form-data" },
      })
        .then((response) => {
          if(response.data.MESSAGE == "ERROR"){
            this.compMain = true
          }else{
            this.compMain = false
            this.btnStore = true
          }
        })
        .catch(function () {
        
        });
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
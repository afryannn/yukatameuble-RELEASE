<template>
  <div>
    <v-overlay :value="loading" :opacity="0.9">
      <v-progress-circular
        indeterminate
        style="color: #28df99"
      ></v-progress-circular>
    </v-overlay>
    <div class="my-header">
      <div class="my-container">
        <div class="my-navbar">
          <div class="my-logo">
            <img src="../assets/b.png" width="180px" />
          </div>
          <nav>
            <ul id="MenuItems">
              <li>
                <router-link to="/">Home</router-link>
              </li>
              <li>
                <router-link to="/Produk" class="hover-c">Produk</router-link>
              </li>
              <li>
                <router-link to="/Cari">Cari</router-link>
              </li>
              <li>
                <router-link to="/Tentang">Tentang</router-link>
              </li>
              <li v-if="this.id == null">
                <router-link to="/Login">Login</router-link>
              </li>
              <li v-else>
                <router-link to="/">{{ this.username }}</router-link>
              </li>
            </ul>
          </nav>
          <!-- <img src="../assets/images/cart.png" width="30px" height="30px"/> -->
          <img src="../assets/images/menu.png" class="menu-icon" />
        </div>
      </div>
    </div>

    <div class="my-small-container single-product">
      <div class="my-row overflow-auto">
        <div class="my-col-2">
          <img
            src="../assets/images/mejatv2.png"
            width="100%"
            id="ProductImg"
          />
          <div class="small-img-row">
            <div class="small-img-col">
              <img
                src="../assets/images/mejatv2.png"
                width="100%"
                class="small-img"
              />
            </div>
            <div class="small-img-col">
              <img
                src="../assets/images/mejatv2.png"
                width="100%"
                class="small-img"
              />
            </div>
            <div class="small-img-col">
              <img
                src="../assets/images/mejatv2.png"
                width="100%"
                class="small-img"
              />
            </div>
            <div class="small-img-col">
              <img
                src="../assets/images/mejatv2.png"
                width="100%"
                class="small-img"
              />
            </div>
          </div>
        </div>
        <div class="my-col-2" v-for="data in datas" v-bind:key="data.id">
          <p>Kategori / {{ data.category }}</p>
          <h1>{{ data.product_name }}</h1>
          <span @click="store(data.store_name)">{{data.store_name}}</span>
          <h4>Rp.{{ data.product_price }}</h4>
          <b-textarea
            v-model="description"
            placeholder="Catatan Untuk Penjual"
            class="txt-area-1 form-control"
          >
          </b-textarea>
           <!-- <img src="../assets/images/wapng.png" style="width:50px !important"> -->
          <b-textarea
            v-model="v_address"
            placeholder="Alamat Lengkap"
            class="txt-area-2 form-control"
          >
          </b-textarea>
          <p v-show="alertAddress" style="color: red; font-size: 13px">
            * Mohon Isi Alamat Lengkap!
          </p>
          <div style="height:10px;"></div>
          <div style=
          "margin-top:15px !important;
          background:white;
          border-style: solid;
          border-width: thin;
          border-radius:15px;
          border-color:#dddddd !important;
          height:35px !important;
          width:180px !important;
           margin:0px !important;">
             <p class="text-center pwa"><img src="../assets/images/wapng.png" style="margin-bottom:2px;width:28px !important">{{data.telephone}}</p>
          </div>
          <Button @click="Buy()" class="btn">Pesan</Button>
          <h3>Deskripsi</h3>
          <p>
            {{ data.description }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      id: "",
      username: "",
      description: "",
      v_address: "",
      alertAddress: false,
      datas: [],
      loading: false,
    };
  },
  methods: {
    store(e){
     var url = "http://localhost:8080/s/" + e;
     window.location.href = url;
    },
    setProduct(data) {
      this.datas = data;
    },
    async Buy() {
      var i = 0;
      var description = this.description;
      let dscVal = "";
      if (description == "") {
        dscVal = "Null";
      } else {
        dscVal = this.description;
      }
      for (i = 0; i < this.datas.length; i++) {
        var store_id = this.datas[i].store_id;
        var store_name = this.datas[i].store_name;
        var product_name = this.datas[i].product_name;
        var product_price = this.datas[i].product_price;
        var address = this.datas[i].address;
        var img1 = this.datas[i].img1;
        var telephone = this.datas[i].telephone;
      }
      if (!this.v_address) {
        this.alertAddress = true;
      } else {
        var bodyFormData = new FormData();
        bodyFormData.append("visitor_id", this.id);
        bodyFormData.append("store_id", store_id);
        bodyFormData.append("store_name", store_name);
        bodyFormData.append("product_name", product_name);
        bodyFormData.append("product_price", product_price);
        bodyFormData.append("product_key", this.$route.params.product_key);
        bodyFormData.append("img1", img1);
        bodyFormData.append("description", dscVal);
        bodyFormData.append("seller_telephone", telephone);
        bodyFormData.append("visitor_telephone",localStorage.getItem('telephone'));
        bodyFormData.append("visitor_name", localStorage.getItem('username'));
        bodyFormData.append("address_seller", address);
        bodyFormData.append("address_customer", this.v_address);
        
        if (localStorage.getItem("role_user") == "SELLER") {
          this.$router.replace({ name: "Dashboard" });
        } else {
          this.loading = true;
          await axios({
            method: "post",
            url: `${this.$api}buy`,
            data: bodyFormData,
            headers: { "Content-Type": "multipart/form-data" },
          })
            .then((res) => {
              console.log(res)
              this.loading = false;
            })
            .catch(function (response) {
              console.log(response);
            });
        }
      }
    },
  },
  mounted() {
    this.loading = true
    this.id = localStorage.getItem("user-id");
    this.username = localStorage.getItem("username");
    axios
      .get(`${this.$api}Detail/` + this.$route.params.product_key)
      .then((response) =>
      this.setProduct(response.data),
      this.loading = false
      )
      .catch((error) => console.log("Fail : ", error));
  },
};
</script>
<style scoped>
.btn{
  margin: 0px !important;
  margin-top: 15px !important;
}
.txt-area-1 {
  width: 25rem;
  height: 6rem;
}
.txt-area-2 {
  margin-top: 10px;
  width: 25rem;
  height: 3rem;
}
.pwa{
  margin-top:3px;
}
@media only screen and (max-width: 800px) {
  .txt-area-1 .txt-area-2 {
    width: 20rem;
  }
}
@media only screen and (max-width: 600px) {
  .txt-area-1 .txt-area-2 {
    width: 17rem;
  }
}
</style>
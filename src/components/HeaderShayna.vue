<template>
  <!-- Header Section Begin -->
  <header class="header-section">
    <div class="header-top">
      <div class="container">
        <div class="ht-left">
          <div class="mail-service">
            <i class="fa fa-envelope"></i> hello.shayna@gmail.com
          </div>
          <div class="phone-service">
            <i class="fa fa-phone"></i> +628 22081996
          </div>
        </div>
      </div>
    </div>
    <div class="container">
      <div class="inner-header">
        <div class="row">
          <div class="col-lg-2 col-md-2">
            <div class="logo">
              <a href="/">
                <img src="img/logo_website_shayna.png" alt="" />
              </a>
            </div>
          </div>
          <div class="col-lg-7 col-md-7"></div>
          <div class="col-lg-3 text-right col-md-3">
            <ul class="nav-right">
              <li class="cart-icon">
                Keranjang Belanja &nbsp;
                <a href="#">
                  <i class="icon_bag_alt"></i>
                  <span>{{ keranjangUser.length }}</span>
                </a>
                <div class="cart-hover">
                  <div class="select-items">
                    <table>
                      <tbody v-if="keranjangUser.length > 0">
                        <tr
                          v-for="keranjang in keranjangUser"
                          :key="keranjang.id"
                        >
                          <td class="si-pic">
                            <img
                              class="photo-card"
                              :src="keranjang.photo"
                              alt=""
                            />
                          </td>
                          <td class="si-text">
                            <div class="product-selected">
                              <p>${{ keranjang.price }} x 1</p>
                              <h6>{{ keranjang.name }}</h6>
                            </div>
                          </td>
                          <td
                            class="si-close"
                            @click="removeItem(keranjangUser.id)"
                          >
                            <i class="ti-close"></i>
                          </td>
                        </tr>
                      </tbody>

                      <tbody v-else>
                        <tr>
                          <td>Keranjang Kosong</td>
                        </tr>
                      </tbody>
                    </table>
                  </div>
                  <div class="select-total">
                    <span>total:</span>
                    <h5>${{ totalHarga }}.00</h5>
                  </div>
                  <div class="select-button">
                    <a href="#" class="primary-btn view-card">
                      <router-link to="/cart" style="color: #fff;">
                        VIEW CARD
                      </router-link>
                    </a>
                    <a href="#" class="primary-btn checkout-btn">CHECK OUT</a>
                  </div>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </header>
  <!-- Header End -->
</template>

<script>
export default {
  name: "HeaderShayna",

  data() {
    return {
      keranjangUser: [],
    };
  },

  methods: {
    removeItem(idx) {
      let keranjangUserStore = JSON.parse(
        localStorage.getItem("keranjangUser")
      );
      let itemKeranjangUserStore = keranjangUserStore.map(
        (itemKeranjangUserStore) => itemKeranjangUserStore.id
      );

      let index = itemKeranjangUserStore.findIndex((id) => id == idx);
      this.keranjangUser.splice(index, 1);

      const parsing = JSON.stringify(this.keranjangUser);
      localStorage.setItem("keranjangUser", parsing);
      window.location.reload();
    },
  },

  mounted() {
    if (localStorage.getItem("keranjangUser")) {
      try {
        this.keranjangUser = JSON.parse(localStorage.getItem("keranjangUser"));
        // Json parse adalah Reviver digunakan untuk memanipulasi value dari json string sebelum diubah menjadi objek. ketika reviver dijalankan, setiap item dari json akan di evaluasi di dalam reviver.
      } catch (error) {
        localStorage.removeItem("keranjangUser");
      }
    }
  },

  computed: {
    totalHarga() {
      return this.keranjangUser.reduce(function (items, data) {
        return items + data.price;
      }, 0);
    },
  },
};
</script>

<style>
.photo-card {
  width: 100px;
  height: 80px;
}
</style>

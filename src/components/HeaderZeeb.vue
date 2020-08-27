<template>
  <header class="header-section">
    <div class="header-top">
      <div class="container">
        <div class="ht-left">
          <div class="mail-service">
            <i class=" fa fa-envelope"></i> zeebme@gmail.com
          </div>
          <div class="phone-service">
            <i class=" fa fa-phone"></i> +628 5771611569
          </div>
        </div>
      </div>
    </div>
    <div class="container">
      <div class="inner-header">
        <div class="row">
          <div class="col-lg-2 col-md-2">
            <div class="logo">
              <a href="./index.html">
                <img src="img/logo_website_zeeb.png" alt="" />
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
                  <span>{{cartUser.length}}</span>
                </a>
                <div class="cart-hover">
                  <div class="select-items">
                    <table>
                      <tbody v-if="cartUser.length > 0">
                        <tr v-for="cart in cartUser" :key="cart.id">
                          <td class="si-pic">
                            <img class="photo-item" :src="cart.photo" alt="" />
                          </td>
                          <td class="si-text">
                            <div class="product-selected">
                              <p>${{cart.price}} x 1</p>
                              <h6>{{cart.name}}</h6>
                            </div>
                          </td>
                          <td @click="removeItem(cartUser.index)" class="si-close">
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
                    <h5>${{totalHarga}}</h5>
                  </div>
                  <div class="select-button">

                   <router-link to="/cart"  class="primary-btn view-card"> <a style="color:#fff;" href="#" >VIEW CARD</a></router-link>
                    <router-link to="/cart" class="primary-btn checkout-btn">CHECK OUT</router-link>
                  </div>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </header>

</template>

<script>
  export default {
    name: 'HeaderZeeb',
    data() {
      return {
        cartUser: []
      };
    },
    methods: {
      removeItem(index) {
        this.cartUser.splice(index, 1);
        const parsed = JSON.stringify(this.cartUser);
        localStorage.setItem('cartUser', parsed);
      }
    },
    mounted() {
      if (localStorage.getItem('cartUser')) {
        try {

          this.cartUser = JSON.parse(localStorage.getItem('cartUser'));
        } catch (e) {
          localStorage.removeItem('cartUser');
        }
      }
    },
    computed:{
      totalHarga(){
        return this.cartUser.reduce(function(items,data){
          return items + data.price;
        }, 0 );
      }
    }
  }
</script>

<style scoped>
  .photo-item {
    width: 80px;
    height: 80px;
  }
</style>
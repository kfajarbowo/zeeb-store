<template>
    <div class="shopping">
        <HeaderZeeb/>

        <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section">
        <div class="container">
            <div class="row">
                <div class="col-lg-12 text-left">
                    <div class="breadcrumb-text product-more ">
                        <router-link to="/"><i class="fa fa-home"></i> Home</router-link>
                        <span>Shopping Cart</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <section class="shopping-cart spad">
        <div class="container">
            <div class="row">
                <div class="col-lg-8">
                    <div class="row">
                        <div class="col-lg-12">
                            <div class="cart-table">
                                <table>
                                    <thead>
                                        <tr>
                                            <th>Image</th>
                                            <th class="p-name text-center">Product Name</th>
                                            <th>Price</th>
                                            <th>Action</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr v-for="cart in cartUser" :key="cart.id">
                                            <td class="cart-pic first-row">
                                                <img class="img-cart" :src="cart.photo" />
                                            </td>
                                            <td class="cart-title first-row text-center">
                                                <h5>{{cart.name}}</h5>
                                            </td>
                                            <td class="p-price first-row">${{cart.price}}</td>
                                            <td @click="removeItem(cartUser.index)" class="delete-item"><a href="#"><i class="material-icons">
                                              close
                                              </i></a></td>
                                        </tr>
                                      
                                    </tbody>
                                </table>
                            </div>
                        </div>
                        <div class="col-lg-8">
                            <h4 class="mb-4 text-left">
                                Informasi Pembeli:
                            </h4>
                            <div class="user-checkout text-left">
                                <form>
                                    <div class="form-group">
                                        <label for="namaLengkap">Nama lengkap</label>
                                        <input v-model="customerInfo.name" type="text" class="form-control" id="namaLengkap" aria-describedby="namaHelp" placeholder="Masukan Nama">
                                    </div>
                                    <div class="form-group">
                                        <label for="namaLengkap">Email Address</label>
                                        <input v-model="customerInfo.email" type="email" class="form-control" id="emailAddress" aria-describedby="emailHelp" placeholder="Masukan Email">
                                    </div>
                                    <div class="form-group">
                                        <label for="namaLengkap">No. HP</label>
                                        <input v-model="customerInfo.number" type="text" class="form-control" id="noHP" aria-describedby="noHPHelp" placeholder="Masukan No. HP">
                                    </div>
                                    <div class="form-group">
                                        <label for="alamatLengkap">Alamat Lengkap</label>
                                        <textarea v-model="customerInfo.address" class="form-control" id="alamatLengkap" rows="3"></textarea>
                                    </div>
                                </form>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-lg-4">
                    <div class="row">
                        <div class="col-lg-12">
                            <div class="proceed-checkout text-left">
                                <ul>
                                    <li class="subtotal ">ID Transaction <span>#SH12000</span></li>
                                    <li class="subtotal mt-3 ">Subtotal <span>${{ totalHarga }}</span></li>
                                    <li class="subtotal mt-3 ">Pajak <span>10% ${{ Pajak }}</span></li>
                                    <li class="subtotal mt-3 ">Total Biaya <span>${{ totalBiaya }}</span></li>
                                    <li class="subtotal mt-3 ">Bank Transfer <span>Mandiri</span></li>
                                    <li class="subtotal mt-3 ">No. Rekening <span>2208 1996 1403</span></li>
                                    <li class="subtotal mt-3 ">Nama Penerima <span>Zeeb</span></li>
                                </ul>
                                <!-- <router-link to="/success" class="proceed-btn">Already Paid</router-link> -->
                                <a href="#" @click="checkout()" class="proceed-btn">Already Paid</a>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>

        
    </div>
    
</template>

<script>
    import HeaderZeeb from "@/components/HeaderZeeb.vue";
    import axios from 'axios';
export default {
    name:'cart',
    components:{
        HeaderZeeb

    },
    data() {
      return {
        cartUser: [],
        customerInfo:{
            name : '',
            email : '',
            number : '',
            address : ''
        }
      };
    },
    methods: {
      removeItem(index) {
        this.cartUser.splice(index, 1);
        const parsed = JSON.stringify(this.cartUser);
        localStorage.setItem('cartUser', parsed);
      },

      //fungsi checkout mengirim data ke API
      checkout(){
          //membuat product menjadi array lalu >
          let productId = this.cartUser.map(function(product){
              return product.id
          });
          //dijadikan ke objek
          let checkoutData = {
              'name' : this.customerInfo.name,
              'email' : this.customerInfo.email,
              'number' : this.customerInfo.number,
              'address' : this.customerInfo.address,
              "transaction_total" : this.totalHarga,
              "transaction_status" : "PENDING",
              "transaction_details" : productId
          };
          axios
          .post("http://shayna-backend.belajarkoding.com/api/checkout", checkoutData)
          .then( ()=> this.$router.push('success'))
          .catch(err => console.log(err));
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
      },
      Pajak(){
          return (this.totalHarga *10) / 100;
      },
      totalBiaya(){
          return this.totalHarga + this.Pajak;
      }
    }
}
</script>

<style scoped>
.img-cart{
    width: 110px;
    height: 110px;
    object-fit: cover;
}
</style>
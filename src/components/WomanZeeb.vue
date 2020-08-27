<template>
    <section class="women-banner spad">
        <div class="container-fluid">
            <div class="row">
                <div class="col-lg-12 mt-5" v-if="products.length > 0">
                    <carousel class="product-slider" :dots='false' :items='3' :nav='false' :autoplay='true'>
                        <div class="product-item" v-for="itemProduct in products" v-bind:key="itemProduct.id">
                            <div class="pi-pic">
                                <img v-bind:src="itemProduct.galleries[0].photo" alt="" style="object-fit:cover; max-height:593px;"  />
                                <ul>
                                    <li class="w-icon active"
                                    @click="saveCart(itemProduct.id, 
                                       itemProduct.name, 
                                       itemProduct.price, 
                                       itemProduct.galleries[0].photo)">
                                        <a href="#" >
                                            <i class="icon_bag_alt"></i>        
                                        </a>
                                    </li>
                                    <li class="quick-view"><router-link v-bind:to="'/product/'+itemProduct.id">+ Quick View</router-link></li>
                                </ul>
                            </div>
                            <div class="pi-text">
                                <div class="catagory-name">{{ itemProduct.type}}</div>
                                <a href="#">
                                    <h5>{{ itemProduct.name}}</h5>
                                </a>
                                <div class="product-price">
                                    ${{itemProduct.price}}
                                    <span>$35.00</span>
                                </div>
                            </div>
                        </div>
                        <!-- <div class="product-item">
                            <div class="pi-pic">
                                <img src="img/products/women-2.jpg" alt="" />
                                <ul>
                                    <li class="w-icon active">
                                        <a href="#"><i class="icon_bag_alt"></i></a>
                                    </li>
                                    <li class="quick-view"><a href="#">+ Quick View</a></li>
                                </ul>
                            </div>
                            <div class="pi-text">
                                <div class="catagory-name">Shoes</div>
                                <a href="#">
                                    <h5>Guangzhou sweater</h5>
                                </a>
                                <div class="product-price">
                                    $13.00
                                </div>
                            </div>
                        </div>
                        <div class="product-item">
                            <div class="pi-pic">
                                <img src="img/products/women-3.jpg" alt="" />
                                <ul>
                                    <li class="w-icon active">
                                        <a href="#"><i class="icon_bag_alt"></i></a>
                                    </li>
                                    <li class="quick-view"><a href="#">+ Quick View</a></li>
                                </ul>
                            </div>
                            <div class="pi-text">
                                <div class="catagory-name">Towel</div>
                                <a href="#">
                                    <h5>Pure Pineapple</h5>
                                </a>
                                <div class="product-price">
                                    $34.00
                                </div>
                            </div>
                        </div>
                        <div class="product-item">
                            <div class="pi-pic">
                                <img src="img/products/man-2.jpg" alt="" />
                                <ul>
                                    <li class="w-icon active">
                                        <a href="#"><i class="icon_bag_alt"></i></a>
                                    </li>
                                    <li class="quick-view"><a href="#">+ Quick View</a></li>
                                    <li class="w-icon">
                                        <a href="#"><i class="fa fa-random"></i></a>
                                    </li>
                                </ul>
                            </div>
                            <div class="pi-text">
                                <div class="catagory-name">Towel</div>
                                <a href="#">
                                    <h5>Converse Shoes</h5>
                                </a>
                                <div class="product-price">
                                    $34.00
                                </div>
                            </div>
                        </div> -->
                    </carousel>
                </div>
                <div class="col-lg-12" v-else>
                    <p>Produk belum tersedia untuk saat ini :)</p>
                </div>
            </div>
        </div>
    </section>
</template>

<script>
import carousel from 'vue-owl-carousel';
import axios from 'axios';
export default {
    name:'WomanZeeb',
    components:{  
        carousel
    },
    data(){
        return{
            products : [],
            cartUser :[]
        };
    },
    mounted(){
        axios
        .get('http://shayna-backend.belajarkoding.com/api/products')
        .then(res => (this.products = res.data.data.data))
        .catch(err => console.log(err));
    },
    methods:{
         saveCart(idProduct, nameProduct, priceProduct, photoProduct){
                var productStored ={
                    "id" : idProduct,
                    "name" : nameProduct,
                    "price" : priceProduct,
                    "photo" :photoProduct
                };
                this.cartUser.push(productStored);
                const parsed = JSON.stringify(this.cartUser);
                localStorage.setItem('cartUser', parsed);
            }

    }
};
</script>

<style scoped>
.product-item{
    margin-right: 25px;
}

</style>
<template>
    <div class="product">
        <HeaderZeeb />
        <!-- Breadcrumb Section Begin -->
        <div class="breacrumb-section text-left">
            <div class="container">
                <div class="row">
                    <div class="col-lg-12">
                        <div class="breadcrumb-text product-more">
                            <router-link to="/"><i class="fa fa-home"></i> Home</router-link>
                            <span>Detail</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <!-- Breadcrumb Section Begin -->

        <!-- Product Shop Section Begin -->
        <section class="product-shop spad page-details">
            <div class="container">
                <div class="row">
                    <div class="col-lg-12">
                        <div class="row">
                            <div class="col-lg-6">
                                <div class="product-pic-zoom">
                                    <img class="product-big-img" :src="gambar_default" alt="" />
                                </div>
                                <div class="product-thumbs" v-if="productDetails.galleries.length > 0">
                                    <carousel class="product-thumbs-track ps-slider" :loop='true' :nav='false'
                                        :dots='false'>
                                        <div v-for="screenshot in productDetails.galleries" :key="screenshot.id"
                                            class="pt" @click="changeImage(screenshot.photo)"
                                            :class="screenshot.photo == gambar_default ? 'active' : '' ">
                                            <img :src="screenshot.photo" alt="" />
                                        </div>

                                        <!-- <div class="pt" @click="changeImage(thumbs[1])" :class="thumbs[1] == gambar_default ? 'active' : '' ">
                                            <img src="img/mickey2.jpg" alt="" />
                                        </div>

                                        <div class="pt" @click="changeImage(thumbs[2])" :class="thumbs[2] == gambar_default ? 'active' : '' ">
                                            <img src="img/mickey3.jpg" alt="" />
                                        </div>

                                        <div class="pt" @click="changeImage(thumbs[3])" :class="thumbs[3] == gambar_default ? 'active' : '' ">
                                            <img src="img/mickey4.jpg" alt="" />
                                        </div> -->
                                    </carousel>
                                </div>
                            </div>
                            <div class="col-lg-6">
                                <div class="product-details text-left">
                                    <div class="pd-title">
                                        <span>{{productDetails.type}}</span>
                                        <h3>{{productDetails.name}}</h3>
                                    </div>
                                    <div class="pd-desc">
                                        <p v-html="productDetails.description"></p>

                                        <h4>${{productDetails.price}}</h4>
                                    </div>
                                    <div class="quantity">
                                        <router-link to="/cart" >
                                       <a href="#" @click="saveCart(productDetails.id, 
                                       productDetails.name, 
                                       productDetails.price, 
                                       productDetails.galleries[0].photo)" 
                                       class="primary-btn pd-cart">Add to Cart</a> 
                                        </router-link>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>
        <!-- Product Shop Section End -->

        <RelatedZeeb />


        <FooterZeeb />
    </div>
</template>

<script>
    import HeaderZeeb from "@/components/HeaderZeeb.vue";
    import RelatedZeeb from "@/components/RelatedZeeb.vue";
    import FooterZeeb from "@/components/FooterZeeb.vue";
    import carousel from 'vue-owl-carousel';
    import axios from 'axios';


    // @ is an alias to /src

    export default {
        name: 'product',
        components: {
            HeaderZeeb,
            FooterZeeb,
            RelatedZeeb,
            carousel
        },
        data() {
            return {
                gambar_default: '',
                productDetails: [],
                cartUser: []
            }
        },

        methods: {
            changeImage(urlImage) {
                this.gambar_default = urlImage;
            },
            setDataPicture(data) {
                //replace object productDetails dengan data dari API
                this.productDetails = data;
                //replace value gambar default dengan data dari API galleries
                this.gambar_default = data.galleries[0].photo;
            },
            saveCart(idProduct, nameProduct, priceProduct, photoProduct){
                var productStored ={
                    "id" : idProduct,
                    "name" : nameProduct,
                    "photo" :photoProduct,
                    "price" : priceProduct
                }
                this.cartUser.push(productStored);
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
            axios
                .get('http://shayna-backend.belajarkoding.com/api/products', {
                    params: {
                        id: this.$route.params.id
                    }
                })
                .then(res => (this.setDataPicture(res.data.data)))
                .catch(err => console.log(err));
        }
    };
</script>

<style scoped>
    .product-thumbs .pt {
        margin-right: 10px;
    }
</style>
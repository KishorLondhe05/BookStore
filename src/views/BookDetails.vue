<script>
import { bookStore } from '../stores/books';
import {Cart} from '../stores/cart'
export default {
    name:'bookDetails',
    data(){
        return{
            bookDetail: null
        }
    },
    methods:{
        addToCart(){
            let cart = Cart();
            cart.addToCart(this.bookDetail)
            alert(`${this.bookDetail.title} is added to cart`)
            this.$router.push({ name: 'home'})
        }
    },
    beforeMount(){
        let books = bookStore()
        let searchedBookName = books.searchedBookName;
        let Books = books.Books[0];

        for(let i=0; i < Books.length; i++){
            if(searchedBookName == Books[i].title){
                books.setBookDetails(Books[i])
                this.bookDetail = books.getBookDetails;
                break
            }
        }

    }
}
</script>

<template>
    <div class="container mt-2 shadow">
        <div class="row p-3">
            <div class="col-md-6">
                <h4 class="mb-2">Book Details</h4>
                <div class="row align-itmes-center">
                    <div class="col-md-12 p-3 d-flex">
                        <img :src="bookDetail.image" alt="img" style="height:20rem;">
                        <div class="mx-3">
                            <p class="title-name">Name:{{ bookDetail.title }} </p>
                            <p class="author">Author: {{ bookDetail.author }} </p>
                            <p class="price">Genre: {{ bookDetail.genre }} </p>
                            <p class="price">Price:₹{{ bookDetail.price }} </p>
                            <button class="btn btn-secondary" @click="addToCart()">Add To Cart</button>
                        </div>
                    </div>
                    <!-- <div class="col-md-6">
                    </div> -->
                </div>
            </div>
            
        </div>
    </div>
</template>
<style>
.title-name{
    color: tomato;
}
.author{
    color: rgb(95, 92, 92)
}
.price{
    color: cornflowerblue
}
</style>

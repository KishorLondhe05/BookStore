<template>
    <div class="container-fluid mt-2">
        <div class="row">
            <div class="col-md-6 d-flex align-items-center">
                <span class="tags ml-0"> Welcome to the book Store</span>
                <!-- <RouterLink to="/cart" class="mr-4"> <button class="btn btn-primary">Cart {{ cartCount }}</button></RouterLink> -->
            </div>
            <div class="col-md-6 justify-content-end">
                <!-- <span class="tags ml-0"> Welcome to the book Store</span> -->
                <RouterLink to="/cart" class="mr-4"> <button class="btn btn-primary">Cart {{ cartCount }}</button></RouterLink>
            </div>
        </div>
         <div v-for="(category, index) in uniqueCategories" :key="index">
            <div class="row bg-light p-2 mt-2" >
                <p class="tags">{{category}}</p>
                <div class="col-md-3 mt-2 mb-1" v-for="(book, index) in categoryWiseBooks[category]" :key="index">
                    <div class="card" style="width: 18rem;" >
                        <img :src="book.image" class="card-img-top img-fluid" style="height: 20rem;" alt="...">
                        <div class="card-body">
                            <div class="row">
                                <div class="col-md-12">
                                    <p class="card-title"><b>{{book.title}}</b></p>
                                </div>
                            </div>
                            <div class="row mb-1">
                                <div class="col-md-8">
                                    <span class="p-0 m-0"><i>{{book.author}}</i></span>
                                </div>
                                <div class="col-md-4">
                                    <p class="p-0 m-0"><b><u>{{book.price}}</u></b></p>
                                </div>
                            </div>
                        <button class="btn btn-primary" @click="gotobookDetails(book.title)">View</button>
                        </div>
                    </div>
                </div>
            </div>
        </div> 
    </div>
</template>

<script>
// import bookStore
// import data from '../assets/data'
import { bookStore } from '../stores/books';
import { Cart } from '../stores/cart';
import { RouterLink, RouterView } from 'vue-router'
export default {
    name:"Dashboard",
    components:{RouterLink},
    data(){
        return{
            Books : [],
            cartCount : 0,
            uniqueCategories : [],
            categoryWiseBooks : {}
        }
    },
    methods:{
        gotobookDetails(bookName){
            console.log(bookName)
            let setter = bookStore();
            setter.setBookName(bookName)
            this.$router.push({ name: 'bookDetails'})
       },
       uniqueFields(){
        for(let i=0; i< this.Books.length; i++){
            if(this.uniqueCategories.indexOf(this.Books[i].genre) === -1){
                this.uniqueCategories.push(this.Books[i].genre);
            }
        }
        console.log(this.uniqueCategories)
        this.uniqueCategories.forEach((category)=>{
            this.categoryWiseBooks[category] = []
            for(let i=0; i< this.Books.length;i++){
                if(category == this.Books[i].genre){
                    this.categoryWiseBooks[category].push(this.Books[i])
                }else{
                    continue;
                }
            }
        })
        console.log(this.categoryWiseBooks)
       }   
    },
    mounted(){
        let books = bookStore()
        let cart = Cart();
        this.cartCount = cart.count
        this.Books = books.Books[0]
        this.uniqueFields()
        console.log(this.uniqueCategories)

    }

}
</script>

<style lang=".scss">
    .tags{
        background-color: #DFCCFB;
        border-radius:4px;
        padding : .3rem;
        color:purple;
        margin-top:1rem
    }
    .lbs{
        font-weight: 600
    }
</style>
<template>
    <div class="row">
        <div class="col-md-12">

            <div class="card mb-3 mt-3" v-for="item in list" :key="item.id">
                <!-- <a class="card-header" v-bind:href="item.slug" v-text="item.title"></a> -->
                <router-link 
                    class="card-header"
                    :to="{name:'post', params:{slug: item.slug}}"
                    v-text="item.title">
                    
                </router-link>
                <div class="card-body">
                    <!-- <p>ID: {{item.id}}</p> -->
                    <p class="card-text" v-text="item.excerpt"></p>
                </div>
            </div>

            <infinite-loading @infinite="infiniteHandler">
                <div slot="no-more">No hay mas datos :D</div>
                <div slot="spinner">Cargando...</div>
                <div slot="no-results">Sin Resuktados</div>
            </infinite-loading>
            
        </div>
    </div>
</template>

<script>
    export default {
       data(){
           return {
               list:[],
               page: 0
           };
       },
       methods:{
           infiniteHandler($state){
               this.page++
               let url = 'http://127.0.0.1:8000/api/posts?page=' + this.page

               axios.get(url).then(response=>{
                   let posts = response.data.data
                   if(posts.length){
                       this.list = this.list.concat(posts)
                       $state.loaded()
                   }else{
                       $state.complete()
                   }
               });
           }
       }
    }
</script>

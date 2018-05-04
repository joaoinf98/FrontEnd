<template>
    <div class="app">
        <div class="listaProdutos">
            <app-ficha-produto v-for="(artigo,index) in artigos" :key="index" :produto="artigo.produto" :valor="artigo.valor" v-on:click.native="carregaCarrinho(artigo)" style="cursor:pointer"/>
        </div>
        <div class="listaCarrinhoCompras">
            <br>
            <h2>Carrinho de Compras</h2>
            {{ultimaCompra}}
            <app-ficha-produto v-for="(artigo,index) in carrinhoCompras" :key="index" :produto="artigo.produto" :valor="artigo.valor" />
        </div>
    </div>
</template>

<script>
    import axios from "axios";
    import appFichaProduto from '@/components/app-ficha-produto';

    export default {
        components: {
            appFichaProduto
        },
        data(){
            return{
                carrinhoCompras:[],
                ultimaCompra: false,
            }
        },
        methods:{
            carregaCarrinho(artigo){
                this.carrinhoCompras.unshift({...artigo, dataCompra: new Date()});
                this.ultimaCompra=this.carrinhoCompras[0].dataCompra;
            }
        },
        async asyncData() {
            return axios.get("https://umaprodutos.firebaseio.com/.json")
            .then((res)=>{
                return{artigos:res.data}
            })
        },
    }
</script>

<style scoped>
    .app{
        margin-top:4.5rem;
        display: flex;
        flex-direction: row;
        justify-content: space-around;
    }
    .listaProdutos{
        display: flex;
        align-items: center;
        flex-direction: column;
    }
    .listaCarrinhoCompras{
        display: flex;
        min-width: 500px;
        align-items: center;
        flex-direction: column;
        border-radius: 5px;
        border: 1px solid #06c4d1;
        background-color: #eee;
    }
</style>




<script setup>

import { ref } from 'vue'

import Swal from 'sweetalert2'

import Produto from './components/Produto.vue';

import ProdutoCarrinho from './components/ProdutoCarrinho.vue';

let showCarrinho = ref(false)

const produtos = ref([
    {
        id: 1,
        nome: 'Camiseta',
        preco: 49.90,
        imagem: 'https://m.media-amazon.com/images/I/415M8ksnThL._AC_SX569_.jpg'
    },
    {
        id: 2,
        nome: 'Casaco',
        preco: 99.90,
        imagem: 'https://m.media-amazon.com/images/I/31sPjQOjWAL._AC_.jpg'
    },
    {
        id: 3,
        nome: 'Boné',
        preco: 9.90,
        imagem: 'https://m.media-amazon.com/images/I/61dn0KdxDfL._AC_SX569_.jpg'
    },
    {
        id: 4,
        nome: 'Calça',
        preco: 199.90,
        imagem: 'https://m.media-amazon.com/images/I/51rY7egceRL._AC_SX569_.jpg'
    },
    {
        id: 5,
        nome: 'Bermuda',
        preco: 29.90,
        imagem: 'https://m.media-amazon.com/images/I/713scHefDuL._AC_SY606_.jpg'
    },
    {
        id: 6,
        nome: 'Meia',
        preco: 99.90,
        imagem: 'https://m.media-amazon.com/images/I/61Nvw6AP6mL._AC_SX569_.jpg'
    },
    {
        id: 7,
        nome: 'Tênis',
        preco: 299.90,
        imagem: 'https://m.media-amazon.com/images/I/51E57XX95kL._AC_SX575_.jpg'
    },
    {
        id: 8,
        nome: 'Relógio',
        preco: 79.90,
        imagem: 'https://m.media-amazon.com/images/I/71LS6dJnP5L._AC_SX679_.jpg'
    },
    {
        id: 9,
        nome: 'Corrente',
        preco: 19.90,
        imagem: 'https://m.media-amazon.com/images/I/51BjvKu-XjL._AC_SX569_.jpg'
    },
])

const produtosCarrinho = ref([
    {
        id: 1,
        quantidade: 10,
        produto: {
            id: 9,
            nome: 'Corrente',
            preco: 19.90,
            imagem: 'https://m.media-amazon.com/images/I/51BjvKu-XjL._AC_SX569_.jpg'
        }
    }
])

produtosCarrinho[Symbol.iterator] = function () {
    let indice = 0;
    const itens = this;

    return {
        next() {
            if (indice < itens.length) {
                return { value: itens[indice++], done: false };
            } else {
                return { done: true };
            }
        }
    };

};

let produtoCarrinhoPosition = ref(0);

function verificarProdutoDuplicado(produto) {
    for (let i = 0; i < produtosCarrinho.value.length; i++) {
        if (produtosCarrinho.value[i].produto.id == produto.id) {
            produtoCarrinhoPosition.value = i;
            return true;
        }
    }
    return false;
}

function addCarrinho(quantidade, produto) {


        if (verificarProdutoDuplicado(produto)) {
            Swal.fire({
                title: 'Produto já encontrado no carrinho',
                text: "Deseja adicionar a quantidade ao carrinho?",
                showCancelButton: true,
                confirmButtonText: 'Sim',
                cancelButtonText: "Cancelar"
            }).then((result) => {
                if (result.isConfirmed) {
                    produtosCarrinho.value[produtoCarrinhoPosition.value].quantidade += quantidade
                } else if (result.isDenied) {
                    console.log('erro')
                }
            })
            return;
        }
            produtosCarrinho.value.push(
                {
                    id: new Date().toISOString().replace(/[-:T.]/g, ''),
                    quantidade: quantidade,
                    produto: produto
                }
            )



    calcularTotal()
}

function deleteProdutoCarrinho(idProduto) {

    for (let i = 0; i < produtosCarrinho.value.length; i++) {
        if (produtosCarrinho.value[i].id == idProduto) {
            produtosCarrinho.value.splice(produtosCarrinho.value.indexOf(produtosCarrinho[i]), 1);
            calcularTotal() 
        }
    }


}

const total = ref(0);

function calcularTotal() {

    total.value = 0;

    for (let i = 0; i < produtosCarrinho.value.length; i++) {

        total.value += Number(produtosCarrinho.value[i].quantidade) * Number(produtosCarrinho.value[i].produto.preco);

    }
}

window.onload = () => {
    calcularTotal()
}
</script>


<template>
    <section class="carrinho" v-if="showCarrinho">
        <span class="closeCarrinho">
            <font-awesome-icon @click="showCarrinho = !showCarrinho" :icon="['fas', 'xmark']" size="xl"
                style="color: #000000;" />
        </span>
        <ProdutoCarrinho v-for="produtoCarrinho in produtosCarrinho" :key="produtoCarrinho.id"
            :produtoCarrinho="produtoCarrinho" @delete-produto-carrinho="deleteProdutoCarrinho" />
        <span class="total">
            R$ {{ total.toFixed(2) }}
        </span>
    </section>

    <main :class="showCarrinho ? 'blur' : ''">
        <div class="home-bar">
            <div class="title">Loja <span>Virtual</span></div>
            <div class="btn-carrinho" @click="showCarrinho = !showCarrinho">
                <button><font-awesome-icon :icon="['fas', 'cart-shopping']" size="xl" style="color: #000000;" /></button>
            </div>
        </div>

        <div class="tabela-produto">
            <Produto v-for="produto in produtos" :produto="produto" :key="produto.id" @add-carrinho="addCarrinho" />
        </div>
    </main>
</template>


<style scoped>
main {
    display: flex;
    justify-content: center;
}

.tabela-produto {
    margin-left: 50vh;
    margin-top: 1vh;
    height: 170vh;
    width: 70%;
    display: flex;
    justify-content: space-around;
    flex-wrap: wrap;
}


.home-bar {
    width: 50vh;
    height: 100vh;
    position: fixed;
    margin-right: 140vh;
    margin-top: 1vh;
}

.title {
    width: 10vh;
    font-family: cursive;
    font-size: 80px;
    color: black;
}

.title>span {
    margin-left: 15vh;
}

.btn-carrinho {
    display: flex;
    justify-content: center;
    margin-top: 5vh;
    filter: blur(0px) !important;
}

.btn-carrinho>button {
    width: 8vh;
    height: 7vh;
    border: 2px rgba(128, 128, 128, 0.418) solid;
    border-radius: 10px;
}

*:focus {
    outline: none;
}

.carrinho {
    position: fixed;
    left: 10%;
    top: 10%;
    width: 80%;
    height: 80vh;
    background-color: rgb(234, 231, 231);
    z-index: 999;
    padding: 1%;
    display: flex;
    flex-wrap: wrap;
    gap: 1%;
    overflow: auto;
    padding-left: 7%;
}

.blur {
    filter: blur(4px);
}

.closeCarrinho {
    position: absolute;
    right: 1%;
    top: 1%;
    cursor: pointer;
}

.total {
    position: absolute;
    bottom: 1%;
    right: 1%;
    background: white;
    padding: 0.5%;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 20px;
    border: solid 1px black;
}
</style>
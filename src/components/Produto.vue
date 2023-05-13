<script setup>
import { ref } from 'vue';

let props = defineProps(["produto"])

let quantidade = ref(0)

</script>

<template>
    <div class="produto">

        <div class="imagem">
            <img :src="props.produto.imagem" alt="">
        </div>

        <p>{{ props.produto.nome }}</p>

        <p>R${{ props.produto.preco }}</p>

        <div class="btn-qnt-produto">
            <input type="number" id="quantidade" min="0" v-model="quantidade">
        </div>

        <button class="btn-carrinho" @click="$emit('add-carrinho', quantidade, props.produto), quantidade = 0">
            Adicionar ao Carrinho
        </button>

    </div>
</template>

<style scoped>
.produto {
    width: 45vh;
    height: 65vh;
    display: flex;
    align-items: center;
    flex-direction: column;
    justify-content: center;
    margin-bottom: 10vh;
    border: black solid 3px;
    border-radius: 5px;
    box-shadow: 1px 1px 5px black;
}

.imagem {
    position: relative;
    bottom: 1vh;
    display: flex;
    justify-content: center;
    width: 45vh;
    height: 40vh;
}

.imagem>img {
    width: 44vh;
    height: 40vh;

}

*:focus {
    outline: none;
}

.btn-qnt-produto>input {
    width: 6vh;
    border: 2px rgba(128, 128, 128, 0.418) solid;
    border-radius: 10px;
    text-align: center;
}

.btn-carrinho {
    width: 50%;
    margin: 1%;
    background-color: transparent;
    border: 2px rgba(128, 128, 128, 0.418) solid;
    border-radius: 10px;
}
</style>
<script setup>
import { ref } from 'vue';

    let props = defineProps(["produtoCarrinho"]);

    let produto = ref(props.produtoCarrinho.produto);
</script>

<template>
    <div class="produto">

        <div class="imagem">
            <img :src="produto.imagem" alt="">
        </div>

        <p>{{ produto.nome }}</p>

        <p>R${{ produto.preco }}</p>

        <p>Quantidade: {{ props.produtoCarrinho.quantidade }}</p>

        <font-awesome-icon class="delete" :icon="['fas', 'trash']" size="xl" style="color: #000000;" @click="$emit('deleteProdutoCarrinho', props.produtoCarrinho.id)" />

    </div>
</template>

<style scoped>
.produto {
    background-color: #ffffff;
    width: 30%;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    border-radius: 20px;
}

.imagem {
    margin: 1%;
    display: flex;
    justify-content: center;
    width: 45vh;
    height: 40vh;
}

.imagem>img {
    width: 44vh;
    height: 40vh;
    border-radius: 20px;
}

*:focus {
    outline: none;
}

.delete {
    cursor: pointer;
}

</style>
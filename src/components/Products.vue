<script setup>
import {ref, computed} from 'vue'

const produtos = ref([])

const nome = ref('')
const preco = ref(0)
const estoque = ref(0)

function adicionar() {
    if(nome.value.trim() === ''){
        alert("Nome nulo")
        return
    }
    produtos.value.push({
        nome : nome.value,
        preco : preco.value,
        estoque : estoque.value
    })
    nome.value = ''
    preco.value = 0
    estoque.value = 0  
    
}

const total = computed(() => {
    return produtos.value.length
})

function deletar(index) {
    produtos.value.splice(index, 1)
}


</script>

<template>
    <div>
        <input type="text" v-model="nome" placeholder="nome">
        <input type="number" v-model="preco" placeholder="preço">
        <input type="number" v-model="estoque" placeholder="estoque">
        <button @click="adicionar">Adicionar</button>
        <ul>
            <li v-for="(produto, index) in produtos" :key="index">Nome: {{ produto.nome }} - Preço:{{ produto.preco }} - Estoque:{{ produto.estoque }} <button @click="deletar(index)">Deletar</button></li>
        </ul>
        <p>Temos {{ total }} produtos</p>
    </div>
</template>


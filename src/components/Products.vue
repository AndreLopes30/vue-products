<script setup>
import {ref, computed, onMounted} from 'vue'

const produtos = ref([])
onMounted(async () => {
    const response = await fetch('http://localhost:3000/produtos')
    const data = await response.json()
    produtos.value = data
})

const nome = ref('')
const preco = ref(0)
const estoque = ref(0)

async function adicionar() {
    if(nome.value.trim() === ''){
        alert("Nome nulo")
        return
    }

    const response = await fetch('http://localhost:3000/produtos', {
        method : 'POST',
        headers : {'Content-Type' : 'application/json'},
        body : JSON.stringify({
            nome : nome.value,
            preco : preco.value,
            estoque : estoque.value
    })})
    const data = await response.json()
    produtos.value.push(data)
    nome.value = ''
    preco.value = 0
    estoque.value = 0  
}

const total = computed(() => {
    return produtos.value.length
})

async function deletar(id) {
    const response = await fetch(`http://localhost:3000/produtos/${id}`, {
        method : 'DELETE'
    })
    produtos.value = produtos.value.filter(produto => produto.id !== id)
}


</script>

<template>
    <div>
        <input type="text" v-model="nome" placeholder="nome">
        <input type="number" v-model="preco" placeholder="preço">
        <input type="number" v-model="estoque" placeholder="estoque">
        <button @click="adicionar">Adicionar</button>
        <ul>
            <li v-for="(produto, index) in produtos" :key="index">Nome: {{ produto.nome }} - Preço:{{ produto.preco }} - Estoque:{{ produto.estoque }} <button @click="deletar(produto.id)">Deletar</button></li>
        </ul>
        <p>Temos {{ total }} produtos</p>
    </div>
</template>


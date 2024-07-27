<script setup>
import { ref, reactive, onMounted } from "vue";
import MarcasApi from "@/api/marcas";
const marcasApi = new MarcasApi();

const defaultMarca = { id: null, marca: "" };
const marcas = ref([]);
const marca = reactive({ ...defaultMarca });

onMounted(async () => {
  marcas.value = await marcasApi.buscarTodasAsMarcas();
});

function limpar() {
  Object.assign(marca, { ...defaultMarca });
}

async function salvar() {
  if (marca.id) {
    await marcasApi.atualizarMarca(marca);
  } else {
    await marcasApi.adicionarMarca(marca);
  }
  marcas.value = await marcasApi.buscarTodasAsMarcas();
  limpar();
}

function editar(marca_para_editar) {
  Object.assign(marca, marca_para_editar);
}

async function excluir(id) {
  await marcasApi.excluirMarca(id);
  marcas.value = await marcasApi.buscarTodasAsMarcas();
  limpar();
}
</script>

<template>
  <h1>Marcas</h1>
  <div class="form">
    <input id="campo" type="text" v-model="marca.nome" placeholder="Nome" />
    <input id="campo" type="text" v-model="marca.nacionalidade" placeholder="Nacionalidade" />
    <button id="b1" @click="salvar">Salvar</button>
    <button id="b2" @click="limpar">Limpar</button>
  </div>
  <ul>
    <li v-for="marca in marcas" :key="marca.id">
      <span @click="editar(marca)">
        ({{ marca.id }}) - {{ marca.nome }} - {{ marca.nacionalidade }}
      </span>
      <button id="b3" @click="excluir(marca.id)">X</button>
    </li>
      
  </ul>
</template>

<style>
#campo{
  border-radius: 10px;
  height: 40px;
  width: 300px;
  background-color: #35578332;
  margin-right: 8px;
  font-size: medium;
}
#b1{
  background-color: #355783;
  border-radius: 10px;
  color: aliceblue;
  height: 40px;
  width: 80px;
}
#b2{
  background-color: #d60606;
  border-radius: 10px;
  color: aliceblue;
  height: 40px;
  width: 80px;
  margin-left: 10px;
}
#b3{
  background-color: #355783;
  border-radius: 10px;
  color: aliceblue;
  height: 40px;
  width: 40px;
  margin-left: 10px;
}
</style>
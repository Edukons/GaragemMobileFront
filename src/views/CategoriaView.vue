<script setup>
import { ref, reactive, onMounted } from "vue";
import CategoriasApi from "@/api/categorias";
const categoriasApi = new CategoriasApi();

const defaultCategoria = { id: null, descricao: "" };
const categorias = ref([]);
const categoria = reactive({ ...defaultCategoria });

onMounted(async () => {
  categorias.value = await categoriasApi.buscarTodasAsCategorias();
});

function limpar() {
  Object.assign(categoria, { ...defaultCategoria });
}

async function salvar() {
  if (categoria.id) {
    await categoriasApi.atualizarCategoria(categoria);
  } else {
    await categoriasApi.adicionarCategoria(categoria);
  }
  categorias.value = await categoriasApi.buscarTodasAsCategorias();
  limpar();
}

function editar(categoria_para_editar) {
  Object.assign(categoria, categoria_para_editar);
}

async function excluir(id) {
  await categoriasApi.excluirCategoria(id);
  categorias.value = await categoriasApi.buscarTodasAsCategorias();
  limpar();
}
</script>

<template>
  <h1>Categoria</h1>
  <div class="form">
    <input id="campo" type="text" v-model="categoria.descricao" placeholder="Descrição" />
    <button id="b1" @click="salvar">Salvar</button>
    <button id="b2" @click="limpar">Limpar</button>
  </div>
  <ul>
    <li v-for="categoria in categorias" :key="categoria.id">
      <span @click="editar(categoria)">
        ({{ categoria.id }}) - {{ categoria.descricao }} -
      </span>
      <button id="b3" @click="excluir(categoria.id)">X</button>
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
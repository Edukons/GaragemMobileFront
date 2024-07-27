<script setup>
import { ref, reactive, onMounted } from "vue";
import CoresApi from "@/api/cores";
const coresApi = new CoresApi();

const defaultCor = { id: null, nome: "" };
const cores = ref([]);
const cor = reactive({ ...defaultCor });

onMounted(async () => {
  cores.value = await coresApi.buscarTodasAsCores();
});

function limpar() {
  Object.assign(cor, { ...defaultCor });
}

async function salvar() {
  if (cor.id) {
    await coresApi.atualizarCor(cor);
  } else {
    await coresApi.adicionarCor(cor);
  }
  cores.value = await coresApi.buscarTodasAsCores();
  limpar();
}

function editar(cor_para_editar) {
  Object.assign(cor, cor_para_editar);
}

async function excluir(id) {
  await coresApi.excluirCor(id);
  cores.value = await coresApi.buscarTodasAsCores();
  limpar();
}
</script>

<template>
  <h1>Cor</h1>
  <div class="form">
    <input id="campo" type="text" v-model="cor.nome" placeholder="Nome" />
    <button id="b1" @click="salvar">Salvar</button>
    <button id="b2" @click="limpar">Limpar</button>
  </div>
  <ul>
    <li v-for="cor in cores" :key="cor.id">
      <span @click="editar(cor)">
        ({{ cor.id }}) - {{ cor.nome }} -
      </span>
      <button id="b3" @click="excluir(cor.id)">X</button>
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
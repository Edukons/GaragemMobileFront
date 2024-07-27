<script setup>
import { ref, reactive, onMounted } from "vue";
import AcessoriosApi from "@/api/acessorios";
const acessoriosApi = new AcessoriosApi();

const defaultAcessorio = { id: null, descricao: "" };
const acessorios = ref([]);
const acessorio = reactive({ ...defaultAcessorio });

onMounted(async () => {
  acessorios.value = await acessoriosApi.buscarTodosOsAcessorios();
});

function limpar() {
  Object.assign(acessorio, { ...defaultAcessorio });
}

async function salvar() {
  if (acessorio.id) {
    await acessoriosApi.atualizarAcessorio(acessorio);
  } else {
    await acessoriosApi.adicionarAcessorio(acessorio);
  }
  acessorios.value = await acessoriosApi.buscarTodosOsAcessorios();
  limpar();
}

function editar(acessorio_para_editar) {
  Object.assign(acessorio, acessorio_para_editar);
}

async function excluir(id) {
  await acessoriosApi.excluirAcessorio(id);
  acessorios.value = await acessoriosApi.buscarTodosOsAcessorios();
  limpar();
}
</script>

<template>
  <h1>Acessorios</h1>
  <div class="form" >
    <input id="campo" type="text" v-model="acessorio.descricao" placeholder="Descrição" />
    <button id="b1" @click="salvar">Salvar</button>
    <button id="b2" @click="limpar">Limpar</button>
  </div>
  <ul>
    <li v-for="acessorio in acessorios" :key="acessorio.id">
      <span @click="editar(acessorio)">
        ({{ acessorio.id }}) - {{ acessorio.descricao }} -
      </span>
      <button id="b3" @click="excluir(acessorio.id)">X</button>
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
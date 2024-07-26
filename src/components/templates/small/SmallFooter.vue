<script setup>
import { ref } from 'vue';
import { onBeforeRouteUpdate } from 'vue-router';

const showMenu = ref(false);

onBeforeRouteUpdate(() => {
  showMenu.value = false;
});
</script>
<template>
<div id="bottom">
  <div id="footerMenu" :style="{ display: showMenu ? 'block' : 'none' }">
    <RouterLink to="/home">
      <i class="mdi mdi-home-outline" />
      Home
    </RouterLink>
    <RouterLink to="/login">
      <i class="mdi mdi-account" />
      Login
    </RouterLink>
  </div>
  <div class="icons">
    <router-link to="/acessorios">
        <i class="icon mdi mdi-account-circle-outline" /> Acessorio
      </router-link>
      <router-link to="/categorias">
        <i class="icon mdi mdi-cart-outline" /> Categoria
      </router-link>
      <router-link to="/cores">
        <i class="icon mdi mdi-cart-outline" /> Cor
      </router-link>
      <router-link to="/marcas">
        <i class="icon mdi mdi-cart-outline" /> Marca
      </router-link>
      <router-link to="/login">
        <i class="icon mdi mdi-account" /> Login
      </router-link>

    <div class="hamburger" @click="showMenu = !showMenu">
      <i class="mdi mdi-menu" />
      Menu
    </div>
  </div>
</div>
</template>
<style scoped>
#footerMenu {
    position: fixed;
    bottom: 15%;
    right: 0;

    width: 20%;
    border-top: #355783 1px solid;
    background-color: #355783;
    display: block;
    padding: 1rem;
}

#footerMenu a {
    display: flex;
    width: 100%;
    justify-content: space-between;
    text-decoration: none;
    color: #ffffff;
    font-size: 1rem;
    transition: color 0.3s;
}


.icons {
    display: flex;
    width: 100%;
    align-items: center;
    justify-content: space-between;
}

.icons a,
.icons .hamburger {
    display: flex;
    flex-direction: column;
    align-items: center;
    text-decoration: none;
    color: #ffffff;
    font-size: 1rem;
    transition: color 0.3s;
}

.hamburger:hover {
    cursor: pointer;
}
#bottom{
  background-color: #355783;
}
</style>
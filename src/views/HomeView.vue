<script setup>
import { onMounted } from 'vue';
import { PassageUser } from '@passageidentity/passage-elements/passage-user';
import { useAuthStore } from '@/stores/auth';

const authStore = useAuthStore();

const getUserInfo = async () => {
  try {
    const authToken = localStorage.getItem('psg_auth_token');
    const passageUser = new PassageUser(authToken);
    const user = await passageUser.userInfo(authToken);
    if (user) {
      await authStore.setToken(authToken);
    } else {
      authStore.unsetToken();
    }
  } catch (error) {
    authStore.unsetToken();
  }
};

onMounted(() => {
  getUserInfo();
});
</script>

<template>
  <h1>Garagem Desenvolvimento Mobile</h1>
  <img src="../assets/logo.png" alt="">
</template>

<style scoped>
h1{
  background-color: #355783;
  border-radius: 20px;
  width: 600px;
  text-align: center;
  margin-left: 530px;
  color: aliceblue;
  box-shadow: 0 0 10px 0 #F29F05;
}
img{
  margin-left: 575px;
}
</style>
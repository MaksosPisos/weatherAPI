<script setup>
import { provide, ref } from 'vue';
import Footer from './components/Footer.vue';
import Header from './components/Header.vue';
import Content from './components/Content.vue';
   const darkTheme = ref(localStorage.darkTheme ? localStorage.darkTheme : "emerald") ;
     const switchTheme = () => {
    darkTheme.value = darkTheme.value === "dark" ? "emerald" : "dark";
    localStorage.darkTheme = darkTheme.value;
  };
provide('theme', darkTheme.value);
provide('switchTheme', switchTheme)
</script>

<template>
  <div :data-theme="darkTheme" class="min-h-screen flex flex-col transition-all text-primary-content">
    <Header>

    </Header>

    <Content>

    </Content>

    <Footer>

    </Footer>
  </div>
</template>

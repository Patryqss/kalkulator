<template>
  <q-layout class="main montserrat" view="lHh Lpr lFf">
    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
export default {
  name: 'MainLayout',
};
</script>

<style lang="scss" scoped>
.main {
  background-color: #e5e5e5;
}
</style>

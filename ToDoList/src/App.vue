<template>
  <div id="app">
    <h1>Список покупок</h1>
    <ShoppingList />
  </div>
</template>

<script>
import ShoppingList from './components/ShoppingList.vue';

export default {
  name: 'App',
  components: {
    ShoppingList,
  },
};
</script>



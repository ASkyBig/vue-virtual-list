<template>
  <div id="app">
    <VirtualList
      :size="40"
      :remain="8"
      :items="items"
      v-slot="defaultSlot"
    >
    <div>{{defaultSlot.item.value}}</div>
    </VirtualList>
  </div>
</template>

<script>
import VirtualList from './components/VirtualList.vue'
const items = [];
for (let i = 0; i < 100; i++) {
  items.push({id: i, value: i})
}

export default {
  name: 'App',
  components: {
    VirtualList
  },
  data () {
    return {
      items: items
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

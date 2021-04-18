<template>
  <div id="app">
    <VirtualList
      :size="10"
      :remain="8"
      :items="items"
      :variable="true"
      v-slot="defaultSlot"
    >
    <div style="border: 1px solid red; padding: 20px 0;">{{defaultSlot.item.value}}</div>
    </VirtualList>
  </div>
</template>

<script>
import VirtualList from './components/VirtualList.vue'
import Mock from 'mockjs'


const items = [];
for (let i = 0; i < 100; i++) {
  items.push({id: i, value: Mock.Random.sentence(5, 50)})
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
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>

<template>
  <div id="app">
    <h2>VueSelectInfiniteScroll demo</h2>
    <VueSelectInfiniteScroll
    :selectedRecord="selectedRecord"
    @fetchData="fetchData"
    />

  </div>
</template>

<script>
import VueSelectInfiniteScroll from './components/select-infinite-scroll.vue'

export default {
  name: 'app',
  components: {
    VueSelectInfiniteScroll
  },
  data(){
    return{
      lastId: 0,
      selectedRecord: {id: 0, label: 'Select...'}
    }
  },
  methods:{
    apimethod(){
      return new Promise((resolve) => {
        let vals = []
        for(var i=1; i< 10; i++){
          vals.push(i)
        }
        let options = vals.map(i => ({
          id: this.lastId + i,
          label: `value-${this.lastId + i}`,
          tooltip: `value-${this.lastId + i}-tooltip`
        }))
        this.lastId = this.lastId + 9

        resolve(options)
      })

    },
    fetchData(params){
      setTimeout(() => {
        this.apimethod(params.q, params.page).then(options => {
        params.done(options)
      })
      }, 2000);


    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

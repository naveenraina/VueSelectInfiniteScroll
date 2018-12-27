# vue-select-infinite-scroll
vue-select-infinite-scroll is an infinite scroll dropdown with filter input for vue.js.

### Install the library
```
npm install vue-select-infinite-scroll --save
```
### Demo
Check this <a href="http://vueinfinitescroll.clickvalley.in" target="_blank">DEMO LINK</a>

### Usage

Make sure bootstrap-vue is used in your application. In main.js (or any startup file in your project) make sure you have following lines -

```
import BootstrapVue from 'bootstrap-vue'
Vue.use(BootstrapVue)
```
And import css files

```
import '../node_modules/bootstrap/dist/css/bootstrap.css';
import '../node_modules/bootstrap-vue/dist/bootstrap-vue.css';
```

Now import the VueSelectInfiniteScroll component -

```
import VueSelectInfiniteScroll from 'vue-select-infinite-scroll/src/components/select-infinite-scroll'

```
Register in components in your vue component -

```
components: {
  VueSelectInfiniteScroll
}
```

Add code in template -

```
<VueSelectInfiniteScroll
  :selectedRecord="selectedRecord"
  @fetchData="fetchData"
/>
```

Add data property -

```
selectedRecord: {id: 0, label: 'Select...'}
```

Add method to return paginated data -

```
fetchData(params){
    YourApiMethod(params.q, params.page).then(options => {

      //callback function
      params.done(options)
  })
}
```

### Dependencies
vue-select-infinite-scroll has following dependencies -
```
bootstrap
bootstrap-vue
vue-infinite-scroll
```

### Github repo
visit <a href="https://github.com/naveenraina/VueSelectInfiniteScroll" target="_blank">git repo</a>

### Sample code

```
import VueSelectInfiniteScroll from 'vue-select-infinite-scroll/src/components/select-infinite-scroll'

export default {
  name: 'app',
  components: {
    HelloWorld,
    VueSelectInfiniteScroll,

  },
  data(){
    return{
      lastId: 0,
      selectedRecord: {id: 0, label: 'Select...'}
    }
  },
  methods:{

    // sample api method to return dummy data
    apimethod(){
      return new Promise((resolve) => {
        let vals = []
        for(var i=1; i< 10; i++){
          vals.push(i)
        }
        let options = vals.map(i => ({
          id: this.lastId + i,
          label: `value-${this.lastId + i}`,
        }))
        this.lastId = this.lastId + 9

        resolve(options)
      })
    },
    fetchData(params){

      //change this call with your real api call
      this.apimethod(params.q, params.page).then(options => {

        // call params.done to push data to select control source
        params.done(options)
      })

    }
  }
}

```
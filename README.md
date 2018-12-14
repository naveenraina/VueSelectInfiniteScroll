# vue-select-infinite-scroll
vue-select-infinite-scroll is an infinite scroll dropdown with filter input for vue.js.

## Install the library
```
npm install vue-select-infinite-scroll --save
```
### Demo
Check this <a href="http://vueinfinitescroll.clickvalley.in" target="_blank">DEMO LINK</a>

### Usage

import the component -

```
import VueSelectInfiniteScroll from './components/select-infinite-scroll.vue'
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


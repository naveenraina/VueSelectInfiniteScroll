<template>
  <b-dropdown :text="selectedRecord.label" class="m-md-2 dd-color" >
    <b-dropdown-header>
      <input v-model="searchtext" placeholder="Search">
      <img v-if="busy === true" src="../assets/loading.gif" height="30px" width="30px">
    </b-dropdown-header>
    <div class="scroll-div" v-infinite-scroll="loadmore" infinite-scroll-distance="10">
      <template v-if="loading === false">
      <b-dropdown-item @click="selectme(option)" :title="getTooltip(option)" v-for="option in options" v-bind:key="option.id">{{option.label}}</b-dropdown-item>
      </template>
      <!-- <template v-if="loading === true">
      <b-dropdown-item >Loading...</b-dropdown-item>
      </template> -->
    </div>
  </b-dropdown>
</template>

<script>
// small change.
import infiniteScroll from 'vue-infinite-scroll'

export default {
  directives: {infiniteScroll},
  props: {
    selectedRecord: Object
  },
  data(){
    return {
      busy: false,
      loading: false,
      disableErraticLoading: true,
      searchtext: '',
      options: [],
      page: 0

    }
  },
  methods:{
    getTooltip(option){
      return option.tooltip ? option.tooltip : option.label
    },
    selectme(record){
      // this.$emit('recordSelected', record)
      this.selectedRecord.id = record.id
      this.selectedRecord.label = record.label
    },
    loadmore(fresh){
      if(this.disableErraticLoading){
        return
      }

      if(this.searchtext === ''){
        this.options = []
        this.page = 0
        return
      }

      this.busy = true;
      if(fresh){
        this.options = []
        this.page = 0
        this.loading = true
      }

      this.$emit('fetchData', {
        done: data => {

          if(this.searchtext === ''){
            this.options = []
            this.page = 0
          } else {
            data.forEach(x => {
              this.options.push(x)
            })
            this.page = this.page + 1
          }

          this.busy = false
          this.loading = false

        },
        q: this.searchtext,
        page: this.page
      })

    }
  },
  watch: {
    searchtext: function(){
      this.disableErraticLoading = false
      this.loadmore(true)
    }
  }
}
</script>
<style>

.scroll-div{
  height: 200px; overflow-y:scroll;
  width: 400px;
  margin: 0 auto;
}

.dropdown-header {
  padding-left: 10px !important;
  padding-right: 40px !important;
}

.dropdown-header img{
  margin-left: 5px;
}

.dropdown-header input{
  width: 100%;
  line-height: 25px;
  font-size: 1rem;
}

.dd-color{
  width: 100%;
}
.dd-color button {
    background-color: white !important;
    color: black !important;

}

.dd-color button:hover{
   background-color: white !important;
    color: black;
}

.dd-color button:focus{
   background-color: white !important;
    color: black;
}

.dropdown-toggle{
  width: 100%;
}

.dropdown-toggle::after{
  float: right;
  margin-top: 10px;
}

</style>
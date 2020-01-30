<template>
  <b-dropdown :text="selectedRecord.label" :style="styling" class="m-md-2 dd-color" variant="outline-secondary" >
    <b-dropdown-header>
      <input v-model="searchtext" placeholder="Search">
      <img v-if="busy === true" src="../assets/loading.gif" height="25px" width="25px">
    </b-dropdown-header>
    <div class="scroll-div scrollbar-primary" v-infinite-scroll="loadmore" infinite-scroll-distance="10">
      <div class="force-overflow">
      <template v-if="loading === false">
      <b-dropdown-item @click="selectme(option)" :title="getTooltip(option)" v-for="option in options" v-bind:key="option.id">{{option.label}}</b-dropdown-item>
      </template>
      </div>
      <!-- <template v-if="loading === true">
      <b-dropdown-item >Loading...</b-dropdown-item>
      </template> -->
    </div>
  </b-dropdown>
</template>

<script>
import infiniteScroll from 'vue-infinite-scroll'

export default {
  directives: {infiniteScroll},
  props: {
    selectedRecord: Object,
    widthInPX: Number
  },
  data(){
    return {
      busy: false,
      loading: false,
      disableErraticLoading: true,
      searchtext: '',
      options: [],
      page: 0,
      delay: 1000
    }
  },
  computed: {
    styling() {
      return {
        width: this.widthInPX + "px"
      };
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
       if (this.timeout) clearTimeout(this.timeout); 
        this.timeout = setTimeout(() => {
          this.disableErraticLoading = false
          this.loadmore(true)
        }, this.delay);
       
    }
  }
}
</script>
<style>
img {
    position: absolute;
    left: 350px;
    top: 18px;
}
.scroll-div{
  height: 200px; overflow-y:scroll;
  width: 400px;
  margin: 0 auto;
}

.scrollbar-primary::-webkit-scrollbar {
  width: 12px;
  background-color: #f5f5f5;
}
.scrollbar-primary::-webkit-scrollbar-thumb {
  border-radius: 10px;
  -webkit-box-shadow: inset 0 0 7px rgba(0, 0, 0, 0.1);
  background-color: #D5E3E8;
}

.dropdown-header {
  padding-left: 10px !important;
  padding-right: 10px !important;
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
    color: #314e4e !important;

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
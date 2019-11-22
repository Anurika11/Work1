<template>
  <div class="container mt-4" style="background-color:#808080; padding:2em;margin:1em;">
    
    <div class="mx-auto">
      <h3>Quest for additional educational programs</h3>
      <div class="form-group">
        <input type="text" class="form-control" placeholder="Enter search text" v-model="searchQuery">
      </div>
      <form class="form-inline row">
        <div class="col-md-4 ">
          <label class="my-1 mr-2 w-100">Channel By Subject Area</label>
          <select class="w-100 custom-select my-1" v-model="subjectArea">
            <option selected>Pick...</option>
            <option value="english">English</option>
            <option value="physics">Physics</option>
            <option value="economics">Economics</option>
          </select>
        </div>
        
        <div class="col-md-4">
          <label class="my-1 mr-2">Filter By Length</label>
          <select class="w-100 custom-select my-1 mr-sm-2" v-model="timeLength">
            <option selected>pick...</option>
            <option value="10">under 10 mins</option>
            <option value="15">under 15 mins</option>
            <option value="30">under 30 mins</option>
            <option value="45">under 45 mins</option>
          </select>
        </div>
        
        <div class="col-md-4">
          <label class="my-1 mr-2">Channel by type</label>
          <select class="w-100 custom-select my-1 mr-sm-2" v-model="type">
            <option selected>Pick...</option>
            <option value="person">Person</option>
            <option value="gathering">Gathering</option>
          </select>
        </div>
        
        <div class="col-md-12">
          <button type="submit" class="btn btn-primary w-75% btn-block  my-4" @click.prevent="search">Search</button>
        </div>
      </form>
      
      <section v-if="results ? results.length : null">
        <nav class="navbar navbar-expand-lg navbar-light bg-light mb-4">
          <a class="navbar-brand" href="#"> {{isResult ? 'Results for ' + searchQuery : 'All classes & activities'}}</a>
          
          <div class="collapse navbar-collapse  ml-auto" id="navbarSupportedContent">
            <div class="navbar-nav ml-auto d-flex align-center">
              <label class="my-1 mr-2">Sort</label>
              <select class="w-100 custom-select my-1 mr-sm-2" v-model="sortVal" @change="sortResult">
                <option selected>Pick...</option>
                <option value="cost" >cost</option>
                <option value="audit" >audit</option>
              </select>
            </div>
          </div>
        </nav>
        <div class="mt-2 row">
          <Result v-for="(result, i) in results" :key="i" :result="result"></Result>
        </div>
      </section>
      
      <section v-else>
        <h3> No Activities or Class found</h3>
      </section>
    
    
    </div>
  </div>
</template>
<script>
    import Result from '@/components/Result';

    export default {
        data() {
            return {
                dropdown: false,
                searchQuery: '',
                subjectArea: '',
                timeLength: '',
                type: '',
                results: null,
                isResult: false,
                sortVal: ''
            }
        },
        components: {
            Result
        },
        methods: {
           
            search() {
                let services = JSON.parse(localStorage.getItem('services'));
                this.results = services.filter(service => service.title.toLowerCase().includes(this.searchQuery.toLowerCase()));

                if (this.results.length >= 1) {
                    this.isResult = true;
                   
                    if (this.subjectArea) {
                        this.results = this.results.filter(result => result.area === this.subjectArea);
                    }

                   
                    if (this.timeLength) {
                        this.results = this.results.filter(result => result.length <= this.timeLength);
                    }

                    //check to apply type filters
                    if (this.type) {
                        this.results = this.results.filter(result => result.orientation === this.type);
                    }

                } else {
                    alert('No class or activity found');
                }
            },
            sortResult(){
                //return alert('hey')
                if(this.sortVal === 'price'){
                    this.results.sort(function(a, b){return a.price - b.price});
                } else{
                    this.results.sort(function(a, b){return b.rating - a.rating });
                }
                
            }
        },
        mounted() {
            this.results = JSON.parse(localStorage.getItem('services'));
        }
    }
</script>
<style>
</style>
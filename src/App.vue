<template>
  <div id="app">
    <NavBar/>
    <NotificationContainer/>
    <router-view :key="$route.fullPath" />
  </div>
</template>

<script>
import NavBar from '@/components/NavBar.vue'
import NotificationContainer from '@/components/NotificationContainer.vue'
import users from "./assets/users.json";
import services from "./assets/services.json";

export default {
  components: {
    NavBar,
    NotificationContainer
  },
  created(){
      //initalize application state on application very first load
    if(!localStorage.getItem('users')){
        localStorage.setItem('users', JSON.stringify(users));
    }
    
    if(!localStorage.getItem('services')){
        localStorage.setItem('services', JSON.stringify(services));
    }
  }
}
</script>


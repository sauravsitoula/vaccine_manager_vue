<template>
      <Button id="add-button"
      @btn-click="toggleAddVaccine"
    />
    <AddVaccine v-show="showAddVaccine" @add-vaccine="addVaccine"></AddVaccine>
  <CardContainer 
  @updateVaccine="toUpdateVaccine" 
  @deleteVaccine="toDeleteVaccine" 
  :vaccines="vaccines"></CardContainer>
</template>

<script>
import axios from "axios";
import {useRouter} from "vue-router";
import CardContainer from "@/components/CardContainer.vue";
import Button from "@/components/Button.vue";
import AddVaccine from "../components/AddVaccine.vue";
import router from "@/router";
import UpdateVaccine from "@/components/UpdateVaccine.vue";

export default {
    name: "Home",
    methods: {
      async updateVaccine(vaccine) {

      },
      async toUpdateVaccine(vaccine) {
        console.log('TO UPDATE')
        router.push('/update/'+vaccine.vaccineId)
        this.toUpdateVaccineValue = vaccine;

      },
      async toDeleteVaccine(vaccineId) {
        console.log(vaccineId);
        axios.defaults.headers.common['Authorization'] = localStorage.getItem('authToken');
        const res= await axios.delete('vaccine/'+vaccineId).then(function(res) {
          console.log(res);
          router.ref('/')
        })
      },
      async addVaccine(vaccine) {
        console.log('vaccine to add ==== ' + JSON.stringify(vaccine))
      // const res = await fetch('http://localhost:1337/vaccine', {
      //   method: 'POST',
      //   headers: {
      //     'Content-type': 'application/json',
      //     'bearer'
      //   },
      //   body: JSON.stringify(vaccine),
      // })
      axios.defaults.headers.common['Authorization'] = localStorage.getItem('authToken');
      console.log( "THE AUTH TOKEN  " + axios.defaults.headers.common['Authorization'])
      const res= await axios.post('vaccine', JSON.stringify(vaccine), {
      headers: {
        "Content-Type": 'application/json'
       }})
       
      this.vaccines = [...this.vaccines, res.data]
    },
      toggleAddVaccine() {
        this.showAddVaccine = !this.showAddVaccine;
      },
      async logout() {
            axios.post("signout", {}, { withCredentials: true }).then(
              function(res) {
                axios.defaults.headers.common["Authorization"] = "";
                //set local storage to null too
                router.push("/login");
              }
            )
            
        },

      async fetchVaccines() {
        try{

       
        axios.defaults.headers.common['Authorization'] = localStorage.getItem('authToken');
        const res= await axios.get('vaccine');
      // const res = await fetch("http://localhost:1337/vaccine")
        // console.log('RES IS' )
      const data = res.data;
      

      return data
    }catch(e) {
          console.log(e);
          router.push('/login')
        }
    },
        
    },
    async created() {
      this.vaccines = await this.fetchVaccines();
      console.log('now vaccine ==')
      console.log(this.vaccines)
      
    },
    mounted() {
      this.$emit('loggedIn');
    },
    data() {
      return {
        vaccines: [],
        showAddVaccine: false,
        toUpdateVaccineValue: {},
      }
    },
    components: { CardContainer, Button, AddVaccine, UpdateVaccine },
    computed: {
    homePage() {
      if (this.$route.path === '/') {
        return true
      } else {
        return false
      }
    },
  },
}
</script>

<style>
  button {
    width: 80px;
  }
  #add-button {
    justify-content: center;
    text-align: center;
  }
</style>

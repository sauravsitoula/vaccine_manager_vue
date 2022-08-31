<template>
    <form @submit="onSubmit" class="update-form">
      <div class="form-control">
        <label>Title</label>
        <input type="text" v-model="title" name="title" placeholder="Add Title" />
      </div>
      <div class="form-control">
        <label>Description</label>
        <input type="text" v-model="description" name="description" placeholder="Add Description" />
      </div>
      <div class="form-control">
        <label>Dose</label>
        <input type="number" v-model="dose" name="dose" placeholder="Add Dose" />
      </div>
      <div class="form-control">
        <label>Image url</label>
        <input type="text" v-model="image" name="image" placeholder="Add Image Link" />
      </div>
      <div class="form-control form-control-check">
        <label>Set mandatory</label>
        <input type="checkbox" v-model="mandatory" name="mandatory" />
      </div>
  
      <input type="submit" value="Update Vaccine" class="btn btn-block" />
    </form>
  </template>
  
  <script>
import router from '@/router';
import axios from 'axios'

  export default {
    name: 'UpdateVaccine',
    async mounted() {
      // console.log()
      axios.defaults.headers.common['Authorization'] = localStorage.getItem('authToken');
      let result = await axios.get('vaccine/'+ this.$route.params.id);
      this.title = result.data.title;
      this.description = result.data.description;
      this.dose = result.data.dose;
      this.image = result.data.image;
      this.mandatory = result.data.mandatory; 
      console.log(result);  
    },
    data() {
      return {
        title: '',
        description: '',
        dose: '',
        image: '',
        mandatory: false,
      }
    },
    methods: {
        addingTitle(value) {
            console.log(value)
        },
        async updateVaccine(updatedVaccine) {
          axios.defaults.headers.common['Authorization'] = localStorage.getItem('authToken');
          const res= await axios.put('vaccine/'+ this.$route.params.id, JSON.stringify(updatedVaccine), {
          headers: {
            "Content-Type": 'application/json'
          }}).then(router.push('/'))
        },
        onSubmit(e) {
          e.preventDefault()
    
          if (!this.title || !this.description || !this.dose || !this.image) {
            alert('Please add all the empty fields')
            return
          }
    
          const updatedVaccine = {
            title: this.title,
            description: this.description,
            dose: this.dose,
            image: this.image,
            mandatory: this.mandatory
          }
          this.updateVaccine(updatedVaccine);
    
        },
    },
    props: {
        updateValue: Object,
  },
  }
  </script>
  
  <style scoped>
  .add-form {
    margin-bottom: 40px;
  }
  
  .form-control {
    margin: 20px 0;
  }
  
  .form-control label {
    display: block;
  }
  
  .form-control input {
    width: 100%;
    height: 40px;
    margin: 5px;
    padding: 3px 7px;
    font-size: 17px;
  }
  
  .form-control-check {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  
  .form-control-check label {
    flex: 1;
  }
  
  .form-control-check input {
    flex: 2;
    height: 20px;
  }
  </style>
  
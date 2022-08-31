<template>
    <div class="card" @mouseover="showAlterButtons" @mouseout = "hideAlterButtons">
      <div class="alter-buttons" v-show="enableAlterButtons">
        <button class="update-button" @click="$emit('updateVaccine', vaccine)">Update</button>
        <button class="delete-button" @click="$emit('deleteVaccine', vaccine.vaccineId)">Delete</button>
      </div>
      <h2>{{vaccine.title}}</h2>
      <div class="image-container">
        <img :src="vaccine.image" />
      </div>
      <p>Description: {{vaccine.description}}</p>
      <p>Number of Dose:  {{vaccine.dose}}</p>
      <p>Mandatory: {{vaccine.mandatory}}</p>
    </div>
</template>

<script>
import Button from './Button.vue';
export default {
    name: "VaccineCard",
    props: {
        vaccine: Object,
    },
    methods: {
      showAlterButtons() {
        this.enableAlterButtons = true;
      },
      hideAlterButtons() {
        this.enableAlterButtons = false;
      }
    },
    data() {
      return {
        enableAlterButtons: false
      }
    },
    components: { Button }
}
</script>

<style scope>
.card {
    background-color: #f4f4f4;
    margin-bottom: 5px;
    border: 1px solid grey;
    border-radius: 1%;
    font-size: small;
}
.fas {
  color: red;
}

.task {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}

.task.reminder {
  border-left: 5px solid green;
}

.task h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.image-container {
    height: 150px;
    width: 200px;
}

img {
    max-width:100%;
    max-height:100%;
}
button {
  width: 70px;
  display: inline;
}
</style>

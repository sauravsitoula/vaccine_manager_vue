<template>
  <div class='card-container' :key="vaccine.vaccineId" v-for="vaccine in vaccines">
    <VaccineCard 
    @updateVaccine="$emit('updateVaccine', vaccine)" 
    @deleteVaccine="$emit('deleteVaccine', vaccine.vaccineId)" 
    :vaccine="vaccine" />
  </div>
</template>

<script>
import VaccineCard from './VaccineCard.vue';

export default {
  name: 'CardContainer',
  props: {
    vaccines: Array,
  },
  components: {
    VaccineCard
},
  emits: ['updateVaccine', 'deleteVaccine']

//   emits: ['updateVaccine', 'deleteVaccine'],
}
</script>

<style>
    .card-container {
        padding-left: 20px;
        padding-right: 20px;
    }
</style>

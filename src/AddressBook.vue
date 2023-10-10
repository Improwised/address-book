
<script setup>
import { reactive,ref } from 'vue'

const refresh = ref(true);

let addresses = reactive([
  {
    name: 'Ridham',
    street1: 'Royal Residency, Barshi',
    street2: 'Royal Residency, Barshi',
    city: 'Barshi',
    phoneNo: 7455899547,
    pincode: 360041
  },
  {
    name: 'Nirav',
    street1: 'Royal Residency, Barshi',
    street2: 'Royal Residency, Barshi',
    city: 'Rajkot',
    phoneNo: 7455899547,
    pincode: 360050
  }
])

function removeItem(index,callback) {
  let temp = addresses[index]
  addresses = addresses.filter((addr) => {
    return addr !== temp
  })
  refresh.value = false;
    callback()

}
function display(){
    console.log(addresses)
     refresh.value = true
}

</script>

<template>

  <table v-if="refresh">
    <thead >
      <tr>
        <td>Name</td>
        <td>street1</td>
        <td>street2</td>
        <td>city</td>
        <td>phone</td>
        <td>pincode</td>
        <td></td>
        <td></td>
      </tr>
    </thead>
    <tr v-for="(address, index) in addresses" :key="index" >
      <td>{{ address.name }}</td>
      <td>{{ address.street1 }}</td>
      <td>{{ address.street2 }}</td>
      <td>{{ address.city }}</td>
      <td>{{ address.phoneNo }}</td>
      <td>{{ address.pincode }}</td>
      <td><button>Edit</button></td>
      <td><button @click="removeItem(index,display)">Delete</button></td>
    </tr>
  </table>

</template>
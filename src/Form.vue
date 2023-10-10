<template>
<!-- {{formData}} -->
  <form class="row g-3 border mt-3 shadow p-5" @submit.prevent="handleSubmit">

    <div class="col-10 text-primary display-6">Add your Deatils</div>
    <button type="button" class="btn-close col-1" aria-label="Close" @click="$emit('showFormvalueChange')"></button>

  <div class="col-md-6">
    <label for="inputName" class="form-label">Name</label>
    <input type="text" class="form-control" id="inputName" v-model="formData.name">
  </div>

  <div class="col-md-6">
    <label for="inputPhone" class="form-label">Phone</label>
    <input type="tel" class="form-control"  id="inputPhone" pattern="[0-9]{3}[0-9]{4}[0-9]{3}" v-model="formData.phoneNumber">
  </div>

  <div class="col-12">
    <label for="inputStreet1" class="form-label">Street1</label>
    <input type="text" class="form-control" id="inputStreet1" v-model="formData.street1">
  </div>

  <div class="col-12">
    <label for="inputStreet2" class="form-label">Street2</label>
    <input type="text" class="form-control" id="inputStreet2" v-model="formData.street2">
  </div>

  <div class="col-md-6">
    <label for="inputCity" class="form-label">City</label>
    <input type="text" class="form-control" id="inputCity" v-model="formData.city">
  </div>
  <div class="col-md-6">
    <label for="inputPincode" class="form-label">Pincode</label>
    <input type="text" class="form-control" id="inputPincode" v-model="formData.pincode">
  </div>

  <div class="col-12">
    <button type="submit" class="btn btn-primary btn-lg" v-text="btnName"></button>
  </div>
</form>

</template>

<script setup>

 import {ref,reactive,computed,defineEmits} from 'vue'

//   const emit = defineEmits('getFormData',formData);
    const emit = defineEmits(['getFormData','showFormvalueChange','getUpdateFromData']);
    const props = defineProps(['updateData'])
    const btnName = ref('add');
 const formData = reactive({
    name:"",
    street1:"",
    street2:"",
    city:"",
    phoneNumber :"",
    pincode:""
 })

   if(props.updateData.id){
    btnName.value = "Update"
    formData.name   = props.updateData.name
    formData.street1   = props.updateData.street1
    formData.street2   = props.updateData.street2
    formData.city   = props.updateData.city
    formData.phoneNumber    = props.updateData.phoneNumber
    formData.pincode   = props.updateData.pincode
   }

 function handleSubmit(){
    if(props.updateData.id){
        emit('getUpdateFromData',formData, props.updateData.id);
    }else{
        emit('getFormData',formData);
    }
    console.log(formData)
    formData.name = "",
    formData.street1 = "",
    formData.street2 = "",
    formData.city = "",
    formData.phoneNumber = "",
    formData.pincode = ""
 }



</script>

<style scoped>

</style>
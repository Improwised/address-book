<template>

  <div class="container">
    <h1 class="mt-3">Address Book</h1>

    <!-- add item button -->
    <div class="row justify-content-end">
        <div v-if="success" class="col-8 alert alert-success" role="alert">{{ success }}</div>
        <div v-if="error" class="col-8 alert alert-danger" role="alert">{{ error }}</div>
        <div class="col-2"></div>
         <div class="col-2">
        <button @click="handleForm('yes')" class="btn btn-primary aligh-right">Add new Address</button>
          </div>

    </div>

    <!-- display table -->
    <div class="row mt-2">
      <div class="col-12">
        <table class="table table-striped table-hover">
          <thead>
            <tr>
                <th scope="col">Sr</th>
                <th scope="col">Name</th>
                <th scope="col">Phone</th>
                <th scope="col">Address</th>
                <th scope="col"></th>
                <th scope="col"></th>
            </tr>


          </thead>
          <tbody v-if="Data.length > 0">
            <tr v-for="(data, index) in Data" :key="index" scope="row">
              <td>{{ index + 1 }}</td>
              <td>{{ data.name }}</td>
              <td>{{ data.phoneNumber }}</td>
              <td>{{ data.street1 }}, {{ data.street2 }}, {{ data.city }} - {{ data.pincode }}</td>
              <td><button @click="handleUpdate(data.id)" class="btn btn-success">Update</button></td>
              <td><button @click="deleteData(data.id)" class="btn btn-danger" data-bs-toggle="modal" data-bs-target="#deleteModal">Delete</button></td>
            </tr>
          </tbody>
          <tfoot v-else>
            <tr>
               <h2> No Data Found</h2>
            </tr>
          </tfoot>
        </table>
      </div>
    </div>

    <!-- modal show -->
      <div class="modal fade" id="deleteModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title fs-5 text-danger" id="exampleModalLabel">Delete</h1>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            Are you sure you want to delete  Address?
          </div>
          <div class="modal-footer">
            <!-- <button type="button" class="btn btn-danger" data-bs-dismiss="modal" @click="deleteData(data.id)">Delete</button> -->
            <button type="button" class="btn btn-danger" data-bs-dismiss="modal" @click="conformDelete()">Delete</button>
          </div>
        </div>
      </div>
    </div>


    <!-- form show -->
    <div class="row">
        <div class="col-12">
            <Form v-if="showForm" @getFormData ="addData" @showFormvalueChange ="handleForm('no')" :updateData ="state"  @getUpdateFromData="updateData" />
        </div>
    </div>
  </div>

</template>

<script setup>
import { ref, reactive, watchEffect } from 'vue'
import Form from './Form.vue'

let url = 'http://localhost:3000/addresses'
let Data = reactive([])
let error = ref('')
let success = ref('')
let showForm = ref(false)
let formDataParent = reactive({})
let state = reactive({});
let conformId = ref();



// get all data
const getAllData = async () => {
  try {
    let res = await fetch(url)
    let resData = await res.json()
    Data.length = 0
    Data.push(...resData)
  } catch (err) {
    error.value = err
    console.log(error.value)
  }
}

// delete data
const deleteData = (id) => conformId.value = id ;

// conformDelete
const conformDelete = async () =>{
    try {
          let res = await fetch(url + '/' + conformId.value, {
            method: 'DELETE'
          })
          if (res.ok != true) {
            throw new Error('not delete something Went wrong!!')
          } else {
            conformDelete.value = false;
            success.value = 'Record Deleted succesfully'
            setTimeout(() => {
              success.value = ''
            }, 3000)
          }
        } catch (err) {
          error.value = err
          console.log(error.value)
          setTimeout(() => {
            error.value = ''
          }, 3000)
        }
        getAllData()

}

//get data
const getData = async (id) => {
  try {
    let res = await fetch(url + "/" + id)
    let resData = await res.json()
    return resData;
  } catch (err) {
    error.value = err
    console.log(error.value)
  }
}

// add form Data
const addData = async (formData) =>{

    formDataParent = {...formData};
        console.log("formData Parent :",formDataParent);
    try {
    let res = await fetch(url, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(formDataParent)
    })
    console.log(res);
    if (res.ok != true) {
      throw new Error('not added something Went wrong!!')
    } else {
        getAllData()
      success.value = 'Record Added succesfully'
      showForm.value = false
      setTimeout(() => {
        success.value = ''
      }, 3000)
    }
  } catch (err) {
    error.value = err
    console.log(error.value)
    setTimeout(() => {
      error.value = ''
    }, 3000)
  }
}

// handleData
const handleUpdate = async (id) =>{

     state = await getData(id);
     handleForm('yes');
    console.log(id);
}

// updateData
const updateData =async (updatedData,id)=>{
     try {
    let res = await fetch(url + "/" + id, {
      method: 'PUT',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(updatedData)
    })
    console.log(res);
    if (res.ok != true) {
      throw new Error('Record not Updated something Went wrong!!')
    } else {
        getAllData()
      success.value = 'Record Updated succesfully'
      showForm.value = false
      state = {}
      setTimeout(() => {
        success.value = ''
      }, 3000)
    }
  } catch (err) {
    error.value = err
    console.log(error.value)
    setTimeout(() => {
      error.value = ''
    }, 3000)
  }
}

// handle Form
const handleForm = (getValue) => {
    console.log('emitted')
    if(getValue === 'no'){
        showForm.value = false
    }
    else{
        showForm.value = true
    }
}

// run on update dom ref
watchEffect(() => {
  if (Data) {
    getAllData()
  }
})

</script>

<style scoped>

</style>
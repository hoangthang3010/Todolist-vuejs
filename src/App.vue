<template>
  <div id="app">
    <div class="row">
      <div class="col-2">
        <span>Employee Details</span>
      </div>
      <div class="col-2 offset-8">
        <button type="button" class="btn btn-primary" @click="clickAdd()">Add New</button>
      </div>
    </div>
    <div class="col-10">
      <table class="table table-bordered">
        <thead>
          <tr>
            <th>Name</th>
            <th>Địa chỉ</th>
            <th>Số điện thoại</th>
            <th class="actions-th">Hành động</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in todos" :key="item.id">
            <td>{{item.name}}</td>
            <td>{{item.address}}</td>
            <td>{{item.phone_number}}</td>
            <td class="actions-td">
              <button type="button" class="btn btn-warning" @click="clickEdit(item)">Sửa</button>
              <button type="button" class="btn btn-danger" @click="clickDelete(item.id)">Xóa</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="container">
      <!-- :member="member" -->
      
      <edit-employee :member="member" @save="clickSave" v-if="isEdit"></edit-employee>
    </div>
  </div>
</template>

<script>

import { ref } from 'vue'
import EditEmployee from './components/EditEmployee'
import axios from 'axios'
export default {
  name: 'App',
  components: { 'edit-employee': EditEmployee },
  setup() {
    const isEdit = ref(false);
    const member = ref([]);
    const todos = ref([])
    const getAllTodos = async () => {
      try {
        const res = await axios.get(
          'https://5feb07678ede8b0017ff234f.mockapi.io/api/todolist/members'
        )
        console.log(res.data);
        todos.value = res.data
      } catch (error) {
        console.log(error)
      }
    }
    const clickAdd = () => {
      let employee = {
         id: Math.floor(Math.random() * 100000),
        name: '',
        address: '',
        phone_number: ''
      }
      member.value = employee
      isEdit.value = true
    }
    const clickEdit = (member1) => {
      isEdit.value = true
      member.value = JSON.parse(JSON.stringify(member1))
    }
    const clickDelete = async id =>{
      try {
        await axios.delete(`https://5feb07678ede8b0017ff234f.mockapi.io/api/todolist/members/${id}`)
        todos.value = todos.value.filter(todo => todo.id !== id)
      } catch (error) {
        console.log(error)
      }
    }
    const clickSave = async () =>{
      console.log(member.value);
      console.log(todos.value);
      let index = todos.value.findIndex(item => item.id == member.value.id)
      console.log(index);
      if(index>=0){
        try {
          await axios.put(
            `https://5feb07678ede8b0017ff234f.mockapi.io/api/todolist/members/${member.value.id}`, member.value
          )
          getAllTodos()
          } catch (error) {
            console.log(error)
        }
      } 
      else{
        try {
          const res = await axios.post(
            'https://5feb07678ede8b0017ff234f.mockapi.io/api/todolist/members/',
            member.value
          )
            todos.value.push(res.data)
          } catch (error) {
            console.log(error)
        }
      }
      isEdit.value = false
    }
    getAllTodos()
    return {
      todos,
      clickDelete,
      clickEdit,
      clickSave,
      isEdit,
      member,
      clickAdd
    }
  }
}

// export default {
//   name: 'App',
//   data(){
//     return {
//       employeeList:[
//         {
//           id:1,
//           name: 'Employee 1',
//           address: 'Tokyo',
//           phone: '0969851742'
//         },
//         {
//           id:2,
//           name: 'Employee 2',
//           address: 'Hà Nội',
//           phone: '0969851777'
//         },
//         {
//           id:3,
//           name: 'Employee 3',
//           address: 'Hawaii',
//           phone: '0969345742'
//         }
//       ],
//       employee: null,
//       isEdit: false
//     }
//   },
//   components: {
//     'edit-employee': EditEmployee
//   },
//   methods: {
//     clickAdd(){
//       let employee = {
//          id: Math.floor(Math.random() * 100000),
//         name: '',
//         address: '',
//         phone: ''
//       }
//       this.employee = employee
//       this.isEdit = true
//     },
//     clickEdit(employee){
//       this.isEdit = true
//       this.employee = JSON.parse(JSON.stringify(employee))
//     },
//     clickSave(employee){
//       let index = this.employeeList.findIndex(item => item.id == employee.id)
//       if(index>=0){
//         this.employeeList.splice(index, 1, employee)
//       } else{
//         this.employeeList.push(employee)
//       }
//       this.isEdit = false
//     },
//     clickDelete(employee){
//        let index = this.employeeList.findIndex(item => item.id == employee.id)
//         this.employeeList.splice(index, 1)
//     }
//   }
// }
</script>
<style>
.col-2{
  text-align: center;
  font-weight: bold;
  font-size: 20px;
  color: #636363;
}
.col-10{
  margin: auto;
}
.row{
  margin-top: 25px;
  margin-bottom: 20px;
  align-items: center;
}
.actions-th{
  width: 20%;
}
.actions-td{
  text-align: center;
}
.actions-td .btn-warning{
  margin-right: 10px;
}
</style>

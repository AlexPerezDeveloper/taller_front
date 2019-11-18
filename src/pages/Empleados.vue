<template>
  <div class="row">
    <div class="col-12">
      <div class="card">
        <!---->
        <div class="card-header">
          <h4 class="card-title">{{title}}</h4>
          <p class="card-category">{{subTitle}}</p>
        </div>
        <!---->
        <div class="table-responsive">
          <table class="table table-striped" v-if="users && users.length">
            <thead>
              <th>Nombre</th>
              <th>Apellidos</th>
              <th>Usuario</th>
              <th>Email</th>
              <th>Puesto</th>
              <th>Creado</th>
            </thead>
            <tbody>
              <tr v-for="user of users">
                <td>{{user.name}}</td>
                <td>{{user.lastname}}</td>
                <td>{{user.username}}</td>
                <td>{{user.email}}</td>
                <td>{{user.positions_name}}</td>
                <td>{{user.created_at}}</td>
                <td>
                  <i class="fa fa-pencil"></i>
                  <button v-on:click="deleteUser(user.id)"><i class="fa fa-trash"></i></button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
        <!---->
      </div>
    </div>

    <div class="flotante">
      <button id="show-modal" @click="showModal = true">Show Modal</button>
      <!-- use the modal component, pass in the prop -->
      <modal v-if="showModal" @close="showModal = false">
        <!--
            you can use custom content here to overwrite
            default content
        -->
        <h3 slot="header">custom header</h3>
      </modal>
    </div>
  </div>
</template>
<script>
import { PaperTable } from "@/components";
import axios from "axios";
import modalUser from "./modalUser";

let token =
  "88e00d15ce7adf6e60155abe3ec66f28uGNTNS6Y1ojyJL62rz4oTD+EcTgPtNxO3qR2o2F7WWOgql68D3EI6EpWIyVsCCGc";

var config = {
  headers: { Authorization: "bearer " + token }
};



export default {
  components: {
    PaperTable,
    modalUser
  },
  data() {
    return {
      title: "Empleados taller",
      subTitle: "Listado de empleados del taller",
      users: [],
      showModal: false
    };
  },

  created() {
    axios
      .get("http://127.0.0.1:3333/api/v1/users/", config)
      .then(response => {
        this.users = response.data;
      })
      .catch(e => {
        this.errors.push(e);
      });
  },
  methods: {
    deleteUser: function (id) {
      axios
      .delete(`http://127.0.0.1:3333/api/v1/users/${id}`, config)
      .then(response => {
        this.result.splice(id, 1)
        console.log(this.result);
      })
      .catch(e => {
        this.errors.push(e);
      });
    }
    
  }
};
</script>




<style>
</style>

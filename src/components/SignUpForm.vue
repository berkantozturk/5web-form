<template>
  <div>
    <form>
      <label> Name</label>
      <input type="text" required v-model="name">
      <label> LastName</label>
      <input type="text" required v-model="lastName">
      <label> Email adress</label>
      <input type="email" required v-model="email">
      <label> Number</label>
      <input type="number" required v-model="number">
      <button @click.prevent="updateRecord" v-if="id != null">Update</button>
      <button @click.prevent="submitForm" v-else>Submit</button>
    </form>

    <table border width="35%">
      <tr>
        <th> Name </th>
        <th> Last Name </th>
        <th> Email </th>
        <th> Number </th>
        <th> Action </th>
      </tr>
      <tbody>
        <tr v-for="person in people" :keys="person.id">
          <td>{{ person.name }}</td>
          <td>{{ person.lastName }}</td>
          <td>{{ person.email }}</td>
          <td>{{ person.number }}</td>
          <td>
            <button v-on:click="fetchFormFieldToUpdate(person)"> Edit </button>
            <button v-on:click="deleteData(person._id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>

import axios from "axios";
export default {
  data() {
    return {
      name: '',
      lastName: '',
      email: '',
      number: '',
      id: null,
      people: []
    };
  },
  created() {
    this.getFormData();

  },
  methods: {
    getFormData() {
      axios.get('http://localhost:3000/products',)
        .then((response) => {
          console.log(response);
          this.people = response.data
        })
        .catch((error) => {
          console.error(error);
        })
    },
    submitForm() {
      const formData = {
        name: this.name,
        lastName: this.lastName,
        email: this.email,
        number: this.number
      };

      axios.post("http://localhost:3000/products", formData, {
        headers: {
          "Access-Control-Allow-Origin": "*",
          Accept: "application/json",
          "Content-Type": "application/json",
        },
        withCredentials: false,
      })
        .then(
          (response) => {
            console.log(response);
          },
          (error) => {
            console.log(error);
          })
    },
    deleteData(id) {
      {
        axios.delete("http://localhost:3000/products/" + id)
          .then(() => {
            this.getFormData();
          })
      }
    },

    fetchFormFieldToUpdate(person) {
      this.name = person.name;
      this.email = person.email;
      this.lastName = person.lastName;
      this.number = person.number;
      this.id = person._id;
    },
    updateRecord() {
      const formData = {
        name: this.name,
        lastName: this.lastName,
        email: this.email,
        number: this.number,
        id: this.id
      };
      axios.put("http://localhost:3000/products", formData, {
        headers: {
          "Access-Control-Allow-Origin": "*",
          Accept: "application/json",
          "Content-Type": "application/json",
        },
        withCredentials: false,
      })
        .then(
          (response) => {
            console.log(response);
          },
          (error) => {
            console.log(error);
          })
    }
  }
}
</script>

<style>
form {
  max-width: 420px;
  margin: 30px auto;
  background: white;
  text-align: left;
  padding: 40px;
  border-radius: 10px;
}

label {
  color: #171414;
  display: inline-block;
  margin: 25px 0 15px;
  font-size: 0.6em;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-weight: bold;
}

input {
  display: block;
  padding: 10px 6px;
  width: 100%;
  box-sizing: border-box;
  border: none;
  border-bottom: 1px solid #ddd;
  color: #555;
}
</style>
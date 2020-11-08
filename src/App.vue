<template>
  <div id="app">
    <form @submit.prevent="handleSubmit">
      <div class="form-control">
        <label for="name">Name</label>
        <input type="text" id="name" v-model="name" />
      </div>
      <div class="form-control">
        <label for="phone">Phone</label>
        <input type="tel" id="phone " v-model="phone" />
      </div>
      <button type="submit">Register</button>
    </form>
    <hr />
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Phone</th>
          <th>Visits</th>
          <th>Last Visit</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="visitor in visitors" :key="visitor.phone">
          <td>{{ visitor.name }}</td>
          <td>{{ visitor.phone }}</td>
          <td>{{ visitor.visits.length }}</td>
          <td>{{ visitor.visits[visitor.visits.length - 1] }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      name: "",
      phone: "",
      visitors: [],
      status: "",
      message: ""
    };
  },
  methods: {
    handleSubmit() {
      console.log("this.name", this.name);
      console.log("this.phone", this.phone);
      fetch("https://optimisten.herokuapp.com/registration", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify({ name: this.name, phone: this.phone })
      })
        .then(res => res.json())
        .then(data => {
          console.log("data", data);
          this.getAllVisitors();
        })
        .catch(err => console.log("err", err));
    },
    getAllVisitors() {
      fetch("https://optimisten.herokuapp.com/registered")
        .then(res => res.json())
        .then(data => (this.visitors = data))
        .catch(err => console.log("err", err));
    }
  },
  beforeMount() {
    this.getAllVisitors();
  }
};
</script>

<style></style>

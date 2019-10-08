<template>
  <div id="workorders">
    <div>{{order.name}}</div>
    <label for="">Description:</label>
    <div class="description">{{order.description}}</div>
    <br>
    <div>Deadline: {{order.deadline}}</div>
    <br>
    <br>
    <ul>
      <li>Worker name: {{order.worker.name}}</li>
      <li>
        <img class="image" v-bind:src="order.worker.image" alt />
      </li>
      <li>Worker Email: {{order.worker.email}}</li>
      <li>Worker Company Name: {{order.worker.companyName}}</li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "workorders",

  props: {
    order: Object
  }
};
</script>

<style>
#workorders {
  border: blue solid 2px;
  margin: 5px;
  width: 500px;
  margin: auto;
}
.description {
  width: 400px;
  margin: auto;
}
li {
  list-style: none;
}
.image {
  width: 100px;
}
</style>

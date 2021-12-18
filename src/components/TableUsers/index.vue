<!-- @format -->

<template>
  <table style="width: 100%">
    <tr>
      <th>Id user</th>
      <th>Account</th>
      <th>Full name</th>
      <th>Email</th>
      <th>Dob</th>
      <th>Role</th>
      <th class="action">Action</th>
    </tr>
    <tr v-for="user in filteredData" :key="user.id">
      <td>{{ user.id }}</td>
      <td>{{ user.account }}</td>
      <td>{{ user.fullname }}</td>
      <td>{{ user.email }}</td>
      <td>{{ user.dob }}</td>
      <td>{{ user.role }}</td>
      <td>
        <button
          class="btn-edit"
          @click="$emit('editUser', { isPopupEdit: true, userEdit: user })"
        >
          Edit
        </button>
        <button
          class="btn-del"
          @click="$emit('deleteUser', { isPopup: true, userId: user.id })"
        >
          Delete
        </button>
      </td>
    </tr>
  </table>
</template>

<script>
  export default {
    name: "TableUsers",
    data() {
      return {};
    },

    props: {
      filteredData: { type: Array },
    },
  };
</script>

<style>
  table,
  th,
  td {
    border: 1px solid black;
  }
  .action {
    width: 200px;
  }
</style>

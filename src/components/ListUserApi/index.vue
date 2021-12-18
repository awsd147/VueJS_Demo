<!-- @format -->

<template>
  <div>
    <h2>List user API</h2>
    <div class="toolbar">
      <input
        type="text"
        v-model="searchUser"
        placeholder="Enter your search terms"
        class="input-search"
      />
      <button class="sort" @click="sort">Sort</button>
      <button class="sort-asc" @click="sortAsc('first_name')">Sort Asc</button>
      <button class="sort-desc" @click="sortDesc('first_name')">
        Sort Desc
      </button>
    </div>
    <div>
      <table style="width: 100%">
        <tr>
          <th>Number</th>
          <th>First name</th>
          <th>Last name</th>
          <th>Email</th>
          <th>Avatar</th>
          Action
        </tr>

        <tr v-for="user in getUser" :key="user.id">
          <td>{{ user.id }}</td>
          <td>{{ user.first_name }}</td>
          <td>{{ user.last_name }}</td>
          <td>{{ user.email }}</td>
          <td><img class="image" :src="user.avatar" /></td>
          <td>
            <button class="btn-edit" @click="editUser(user)">Edit</button>
            <button class="btn-del" @click="deleteUser(user.id)">Delete</button>
          </td>
        </tr>
      </table>
    </div>
    <PopupDelete
      :isPopupDeleteApi="isPopupDeleteApi"
      :userId="userId"
      @deleteUserPopupApi="deleteUserPopupApi"
      @closeDeleteUserPopupApi="closeDeleteUserPopupApi"
    />

    <PopupEdit
      :isPopupEditApi="isPopupEditApi"
      :userEdit="userEdit"
      @closePopupEditApi="closePopupEditApi"
      @getUserEditedApi="getUserEditedApi"
    />
  </div>
</template>

<script>
  import axios from "axios";
  import PopupDelete from "../PopupDelete";
  import PopupEdit from "../PopupEdit";
  export default {
    name: "ListUsers",
    components: {
      PopupDelete,
      PopupEdit,
    },
    data() {
      return {
        searchUser: "",
        data: [],
        isPopupDeleteApi: false,
        isPopupEditApi: false,
        userId: "",
        userEdit: {},
      };
    },
    methods: {
      sort() {
        this.data = this.data.reverse();
      },
      sortAsc(key) {
        this.data.sort((a, b) => (a[key] > b[key] ? 1 : -1));
      },
      sortDesc(key) {
        this.data.sort((a, b) => (a[key] < b[key] ? 1 : -1));
      },
      deleteUser(userId) {
        this.isPopupDeleteApi = !this.isPopupDeleteApi;
        this.userId = userId;
      },
      deleteUserPopupApi(userId) {
        this.isPopupDeleteApi = !this.isPopupDeleteApi;
        this.data = this.data.filter((user) => user.id !== userId);
      },
      closeDeleteUserPopupApi() {
        this.isPopupDeleteApi = !this.isPopupDeleteApi;
      },
      editUser(user) {
        this.isPopupEditApi = !this.isPopupEditApi;
        this.userEdit = user;
      },
      closePopupEditApi() {
        this.isPopupEditApi = !this.isPopupEditApi;
      },
      getUserEditedApi(userEdit) {
        this.data = this.data.filter((user) => user.id !== userEdit.id);
        this.data.unshift(userEdit);
        this.isPopupEditApi = !this.isPopupEditApi;
      },
    },
    computed: {
      getUser: function () {
        let users = this.data;
        let keySearch = this.searchUser;
        !keySearch && users;

        keySearch = keySearch.trim().toLowerCase();
        users = users.filter((item) => {
          if (item.last_name.toLowerCase().indexOf(keySearch) !== -1) {
            return item;
          }
        });
        return users;
      },
    },
    mounted() {
      axios
        .get("https://reqres.in/api/users")
        .then((response) => (this.data = response.data.data));
    },
  };
</script>

<style>
  table,
  th,
  td {
    border: 1px solid black;
  }
  .toolbar {
    margin-bottom: 10px;
    display: flex;
  }
  .input-search {
    height: 35px;
    width: 60%;
    border-radius: 10px;
    border: 1px solid #d19e9e;
  }
  .sort,
  .sort-asc,
  .sort-desc {
    margin-left: 10px;
    width: 80px;
    height: 40px;
    border-radius: 10px;
    border: 1px solid #ffff;
  }
  .btn-del {
    background: #f40337;
  }
  .btn-edit {
    background: #03a9f4;
  }
  .btn-del,
  .btn-edit {
    width: 70px;
    margin: 5px;
    padding: 8px 13px;
    border: 1px solid #ffff;
    border-radius: 10px;
    color: #ffff;
    transition: all 0.3s ease;
  }
  .btn-del:hover,
  .btn-edit:hover {
    cursor: pointer;
    opacity: 0.7;
  }
  .image {
    width: 200px;
    height: 200px;
  }
</style>

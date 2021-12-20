<!-- @format -->

<template>
  <div id="app">
    <FormMember @addUser="addUser" />
    <h1>List User</h1>
    <div class="option">
      <input
        type="text"
        v-model="searchString"
        placeholder="Enter your search terms"
        class="input-search"
      />
      <button class="sort" @click="sort">Sort</button>
      <button class="sort-asc" @click="sortAsc('email')">Sort Asc</button>
      <button class="sort-desc" @click="sortDesc('email')">Sort Desc</button>
    </div>
    <TableUsers
      @deleteUser="deleteUser"
      @editUser="editUser"
      :filteredData="filteredData"
    />
    <PopupDelete
      :isPopup="isPopup"
      :userId="userId"
      @deleteUserPopup="deleteUserPopup"
      @closeDeleteUserPopup="closeDeleteUserPopup"
    />
    <PopupEdit
      :isPopupEdit="isPopupEdit"
      :userEdit="userEdit"
      @closePopupEdit="closePopupEdit"
      @getUserEdited="getUserEdited"
    />
    <ListUserApi />
  </div>
</template>

<script>
  import FormMember from "./components/FormMember";
  import TableUsers from "./components/TableUsers";
  import PopupDelete from "./components/PopupDelete";
  import PopupEdit from "./components/PopupEdit";
  import ListUserApi from "./components/ListUserApi";
  export default {
    name: "App",
    components: { FormMember, TableUsers, PopupDelete, PopupEdit, ListUserApi },
    data() {
      return {
        users: [
          // {
          //   fullname: "George",
          // },
          // {
          //   fullname: "Janet",
          // },
          // {
          //   fullname: "Emma",
          // },
          // {
          //   fullname: "Eve",
          // },
          // {
          //   fullname: "Charles",
          // },
          // {
          //   fullname: "Tracey",
          // },
        ],
        isPopupEdit: false,
        isPopup: false,
        userId: "",
        userEdit: {},
        searchString: "",
      };
    },
    methods: {
      addUser(users) {
        this.users.push(users);
      },
      deleteUser(props) {
        this.userId = props.userId;
        this.isPopup = props.isPopup;
      },
      deleteUserPopup(props) {
        const { userId, isPopup } = props;
        this.isPopup = !isPopup;
        this.users = this.users.filter((user) => user.id !== userId);
      },
      closeDeleteUserPopup() {
        this.isPopup = false;
      },
      editUser(props) {
        const { userEdit, isPopupEdit } = props;
        this.userEdit = userEdit;
        this.isPopupEdit = isPopupEdit;
      },
      closePopupEdit() {
        this.userEdit = { ...this.userEdit };
        this.isPopupEdit = false;
      },
      getUserEdited(props) {
        const { userEdit, isPopupEdit } = props;
        this.users = this.users.filter((user) => user.id !== userEdit.id);
        this.users.unshift(userEdit);
        this.isPopupEdit = isPopupEdit;
      },
      sort() {
        this.users = this.users.reverse();
      },
      sortAsc(key) {
        this.users.sort((a, b) => (a[key] > b[key] ? 1 : -1));
      },
      sortDesc(key) {
        this.users.sort((a, b) => (a[key] < b[key] ? 1 : -1));
      },
    },
    computed: {
      filteredData: function () {
        var search_array = this.users,
          searchString = this.searchString;

        if (!searchString) {
          return search_array;
        }

        searchString = searchString.trim().toLowerCase();

        search_array = search_array.filter((item) => {
          if (item.email.toLowerCase().indexOf(searchString) !== -1) {
            return item;
          }
        });
        return search_array;
      },
    },
  };
</script>

<style>
  * {
    margin: 0;
    padding: 0;
  }
  #app {
    padding: 10px;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    background: #c6ffdd;
    background: -webkit-linear-gradient(to left, #f7797d, #fbd786, #c6ffdd);
    background: linear-gradient(to left, #f7797d, #fbd786, #c6ffdd);
  }
  .option {
    display: flex;
    margin-bottom: 10px;
  }
</style>

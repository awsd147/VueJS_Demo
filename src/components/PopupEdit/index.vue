<!-- @format -->

<template>
  <div
    class="popup"
    :class="{
      hide:
        !isPopupEdit & (isPopupEditApi == false)
          ? true
          : !isPopupEditApi & (isPopupEdit == false)
          ? true
          : false,
    }"
  >
    <div class="content">
      <form @submit.prevent="editUser" v-if="isPopupEdit">
        <div class="form">
          <h4>Account</h4>
          <input
            type="text"
            name="account"
            @input="account = $event.target.value"
            :value="userEdit.account"
          />
        </div>
        <div class="form">
          <h4>Full name</h4>
          <input
            type="text"
            @input="fullname = $event.target.value"
            :value="userEdit.fullname"
          />
        </div>
        <div class="form">
          <h4>Email</h4>
          <input
            type="text"
            @input="email = $event.target.value"
            :value="userEdit.email"
          />
        </div>
        <div class="form">
          <h4>Date of birth</h4>
          <input
            type="date"
            name="dob"
            @input="dob = $event.target.value"
            :value="userEdit.dob"
            id="myDate"
          />
        </div>
        <div class="form">
          <h4>Role</h4>
          <select
            name="admin"
            id="admin"
            @input="role = $event.target.value"
            :value="userEdit.role"
          >
            <option value="admin">admin</option>
            <option value="user">user</option>
          </select>
        </div>
        <button type="submit" class="btn-edit">Edit</button>
      </form>
      <form @submit.prevent="editUserApi" v-if="isPopupEditApi">
        <div class="form">
          <h4>first name</h4>
          <input
            type="text"
            @input="first_name = $event.target.value"
            :value="userEdit.first_name"
          />
        </div>
        <div class="form">
          <h4>Last name</h4>
          <input
            type="text"
            @input="last_name = $event.target.value"
            :value="userEdit.last_name"
          />
        </div>

        <div class="form">
          <h4>Email</h4>
          <input
            type="text"
            @input="email = $event.target.value"
            :value="userEdit.email"
          />
        </div>
        <div class="form avatar">
          <h4>Avatar</h4>
          <img
            :src="avatar == '' ? userEdit.avatar : avatar"
            @click="chooseAvatar"
          />
          <input type="file" ref="chooseAvatar" @change="chooseAvatar" />
        </div>
        <button type="submit" class="btn-edit">Edit</button>
      </form>
      <button
        @click="$emit('closePopupEdit')"
        class="btn-close"
        v-if="isPopupEdit"
      >
        Cancel
      </button>
      <button
        @click="closePopupEditApi"
        class="btn-close1"
        v-if="isPopupEditApi"
      >
        Cancel
      </button>
    </div>
  </div>
</template>

<script>
  export default {
    name: "PopupEdit",
    data() {
      return {
        account: "",
        email: "",
        fullname: "",
        dob: "",
        role: "",
        first_name: "",
        last_name: "",
        avatar: "",
      };
    },
    props: {
      isPopupEditApi: { type: Boolean, default: false },
      isPopupEdit: { type: Boolean, default: false },
      userEdit: { type: Object },
    },
    methods: {
      editUser() {
        const { account, fullname, dob, email, role } = this;
        const userEdit = {
          id: this._props.userEdit.id,
          account: account == "" ? this._props.userEdit.account : account,
          fullname: fullname == "" ? this._props.userEdit.fullname : fullname,
          email: email == "" ? this._props.userEdit.email : email,
          dob: dob == "" ? this._props.userEdit.dob : dob,
          role: role == "" ? this._props.userEdit.role : role,
        };
        this.$emit("getUserEdited", { isPopupEdit: false, userEdit: userEdit });
      },
      editUserApi() {
        const { first_name, email, last_name, avatar } = this;
        const userEdit = {
          id: this._props.userEdit.id,
          first_name:
            first_name == "" ? this._props.userEdit.first_name : first_name,
          last_name:
            last_name == "" ? this._props.userEdit.last_name : last_name,
          email: email == "" ? this._props.userEdit.email : email,
          avatar: avatar == "" ? this._props.userEdit.avatar : avatar,
        };
        this.$emit("getUserEditedApi", userEdit);
        this.avatar = "";
      },
      closePopupEditApi() {
        this.$emit("closePopupEditApi");
        this.avatar = "";
      },
      chooseAvatar(e) {
        this.$refs.chooseAvatar.click();
        if (e.target.files)
          this.avatar = URL.createObjectURL(e.target.files[0]);
      },
    },
  };
</script>

<style scoped>
  .popup {
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
  }
  .content {
    width: 280px;
    padding: 25px;
    position: absolute;
    left: 50%;
    top: 35%;
    transform: translate(-50%, -50%);
    background-color: azure;
    border: 1px solid gainsboro;
    border-radius: 5px;
  }
  .content p {
    text-align: left;
  }
  .content button {
    padding: 8px 10px;
    margin-top: 10px;
    background-color: antiquewhite;
    border: 1px solid yellow;
    font-family: Arial, Helvetica, sans-serif;
    transition: all 0.4s ease;
  }
  .content button:hover {
    cursor: pointer;
    background-color: darkgrey;
    color: aliceblue;
    border: 1px solid grey;
  }
  .hide {
    display: none;
  }
  .form {
    display: flex;
    margin: 15px 0;
  }
  h4 {
    width: 85px;
    margin: 0;
  }
  .form input,
  .form select {
    border: 1px solid rgb(114, 110, 110);
    border-radius: 6px;
  }
  .btn-edit {
    margin-right: 50%;
    width: 100px;
  }
  .btn-close {
    width: 100px;
    position: absolute;
    top: 76%;
    right: 55px;
    border-radius: 10px;
  }
  .btn-close1 {
    width: 100px;
    position: absolute;
    top: 68%;
    right: 55px;
    top: 75%;
    border-radius: 10px;
  }
  .avatar img {
    width: 55px;
    height: 55px;
    border-radius: 45px;
    border: 1px solid gray;
    transition: all 0.3s ease;
  }
  .avatar img:hover {
    cursor: pointer;
    opacity: 0.8;
  }
  .avatar input {
    display: none;
  }
</style>

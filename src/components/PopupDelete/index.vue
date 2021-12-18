<!-- @format -->

<template>
  <div
    class="popup"
    v-bind:class="{
      hide:
        !isPopup & (isPopupDeleteApi == false)
          ? true
          : !isPopupDeleteApi & (isPopup == false)
          ? true
          : false,
    }"
  >
    <div class="content">
      <h1>Delete user</h1>
      <p>Do you want delete this user ?</p>
      <button @click="deleteUserPopup" v-if="isPopup">Delete user</button>
      <button @click="deleteUserPopupApi" v-if="isPopupDeleteApi">
        Delete user
      </button>
      <button @click="$emit('closeDeleteUserPopup')" v-if="isPopup">
        Cancel
      </button>
      <button @click="$emit('closeDeleteUserPopupApi')" v-if="isPopupDeleteApi">
        Cancel
      </button>
    </div>
  </div>
</template>

<script>
  export default {
    name: "PopupDelete",
    data() {
      return {};
    },
    props: {
      isPopup: { type: Boolean, default: false },
      userId: { type: [String, Number], default: "" },
      isPopupDeleteApi: { type: Boolean, default: false },
    },
    methods: {
      deleteUserPopup() {
        this.$emit("deleteUserPopup", this._props);
      },
      deleteUserPopupApi() {
        this.$emit("deleteUserPopupApi", this._props.userId);
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
    width: 265px;
    padding: 25px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    background-color: azure;
    border: 1px solid gainsboro;
    border-radius: 5px;
  }
  .content p {
    text-align: left;
  }
  .content button {
    margin: 0 25px;
    padding: 8px 10px;
    margin-top: 10px;
    background-color: red;
    border: 1px solid orange;
    font-family: Arial, Helvetica, sans-serif;
    transition: all 0.4s ease;
    color: #ffff;
  }
  .content button:hover {
    cursor: pointer;
    background-color: darkgrey;
    color: black;
    border: 1px solid grey;
  }
  .hide,
  .hide1 {
    display: none;
  }
  .content h1 {
    color: red;
    margin: 0;
  }
  .content p {
    text-align: center;
  }
</style>

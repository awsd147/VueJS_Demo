<!-- @format -->

<template>
  <form @submit.prevent="addMember" class="form">
    <div>
      <h4>Account</h4>
      <input
        placeholder="Account"
        :class="{
          'is-invalid': validation.invalid.account,
        }"
        @focus="clearValidation('account')"
        v-model="account"
      />

      <div class="tooltip" v-if="validation.invalid.account">
        <span class="tooltiptext"> {{ validation.invalid.account }}</span>
      </div>
    </div>
    <div>
      <h4>Full name</h4>
      <input
        placeholder="Full name"
        :class="{
          'is-invalid': validation.invalid.fullname,
        }"
        @focus="clearValidation('fullname')"
        v-model="fullname"
      />

      <div class="tooltip" v-if="validation.invalid.fullname">
        <span class="tooltiptext"> {{ validation.invalid.fullname }}</span>
      </div>
    </div>

    <div>
      <h4>Email</h4>
      <input
        type="email"
        placeholder="Email"
        :class="{
          'is-invalid': validation.invalid.email,
        }"
        @focus="clearValidation('email')"
        v-model="email"
      />

      <div class="tooltip" v-if="validation.invalid.email">
        <span class="tooltiptext"> {{ validation.invalid.email }}</span>
      </div>
    </div>
    <div>
      <h4>Date of birth</h4>
      <input
        type="date"
        placeholder="Date of birth"
        :class="{
          'is-invalid': validation.invalid.dob,
        }"
        @focus="clearValidation('dob')"
        v-model="dob"
        id="myDate"
      />
      <div class="tooltip" v-if="validation.invalid.dob">
        <span class="tooltiptext"> {{ validation.invalid.dob }}</span>
      </div>
    </div>
    <div>
      <h4>Role</h4>
      <select
        :class="{
          'is-invalid': validation.invalid.role,
        }"
        @focus="clearValidation('role')"
        v-model="role"
      >
        <option disabled value="">Select role</option>
        <option value="admin">admin</option>
        <option value="user">user</option>
      </select>
      <div class="tooltip" v-if="validation.invalid.role">
        <span class="tooltiptext"> {{ validation.invalid.role }}</span>
      </div>
    </div>
    <button class="btn-add" type="submit">Add</button>
  </form>
</template>

<script>
  import { v4 as uuidv4 } from "uuid";
  export default {
    name: "FormMember",
    data() {
      return {
        account: "",
        email: "",
        fullname: "",
        dob: "",
        role: "",
        validation: {
          invalid: {},
          valid: {},
        },
      };
    },
    methods: {
      addMember() {
        let { account, fullname, dob, email, role } = this;
        const user = {
          id: uuidv4()?.split("-")[0],
          account: account,
          fullname: fullname,
          email: email,
          dob: dob,
          role: role,
        };

        !this.account
          ? (this.validation.invalid.account = "Please type your Account.")
          : this.account.length < 8
          ? (this.validation.invalid.account =
              "Account should have min. 8 characters.")
          : (this.validation.invalid.account = "");

        !this.fullname
          ? (this.validation.invalid.fullname = "Please type your Full Name.")
          : this.fullname.length < 5
          ? (this.validation.invalid.fullname =
              "Full Name should have min. 6 characters.")
          : (this.validation.invalid.fullname = "");

        !this.email
          ? (this.validation.invalid.email = "Please type your Email.")
          : (this.validation.invalid.email = "");

        !this.dob
          ? (this.validation.invalid.dob = "Please type your Date of birth.")
          : (this.validation.invalid.dob = "");
        !this.role
          ? (this.validation.invalid.role = "Please select your role.")
          : (this.validation.invalid.role = "");

        this.$forceUpdate();

        if (this.account.length >= 8 && this.fullname.length >= 5) {
          this.$emit("addUser", user);
          this.account = this.fullname = this.email = this.dob = this.role = "";
        }
      },
      clearValidation: function (field) {
        this.validation.invalid[field] = "";
        this.$forceUpdate();
      },
    },
  };
</script>

<style scoped>
  .form {
    max-width: 400px;
    padding: 15px 35px 45px;
    background: #74ebd5;
    background: -webkit-linear-gradient(to right, #acb6e5, #74ebd5);
    background: linear-gradient(to right, #acb6e5, #74ebd5);
    border-radius: 10px;

    margin: 0 auto;
    border: 1px solid rgba(0, 0, 0, 0.1);
    text-align: left;
  }
  .form h4 {
    padding: 0;
    margin: 10px;
  }
  .form input {
    border-radius: 10px;
    height: 30px;
    padding-left: 5px;
  }
  .btn-add {
    margin: 30px 20px;
    padding: 10px 20px;
    border-radius: 12px;
    border: 1px solid salmon;
    transition: all 0.3s ease;
    color: #ffff;
    background-color: cadetblue;
  }
  .btn-add:hover {
    color: black;
    opacity: 0.9;
  }
  .is-invalid {
    border: 2px solid red;
  }
  .tooltip {
    position: relative;
    display: inline-block;
    border-bottom: 1px dotted black;
  }

  .tooltip .tooltiptext {
    text-align: center;
    border: 1px solid red;
    width: 225px;
    background-color: #ffff;
    color: red;
    border-radius: 6px;
    padding: 2px 0;
    position: absolute;
    margin-left: 10px;
    top: -16px;
  }

  .tooltip .tooltiptext::after {
    content: " ";
    position: absolute;
    top: 50%;
    right: 100%;
    margin-top: -5px;
    border-width: 5px;
    border-style: solid;
    border-color: transparent red transparent transparent;
  }

  .tooltip:hover .tooltiptext {
    visibility: visible;
  }
</style>

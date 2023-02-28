<template>
  <transition name="modal">
    <div class="layer">
      <div class="modal-wrapper">
        <div class="modal-container">
          <ValidationObserver v-slot="{ handleSubmit }">
            <form @submit.prevent="handleSubmit(onSubmit)">
              <ValidationProvider
                name="Security-Series"
                rules="required"
                v-slot="{ errors }"
              >
                <label>Select Security Class </label>
                <select name="series" v-model="nameSelected">
                  <option disabled value="">Please select one</option>
                  <option
                    v-for="serie in options.series"
                    :key="serie.value"
                    :value="serie.text"
                  >
                    {{ serie.text }}
                  </option>
                </select>
                <span>{{ errors[0] }}</span>
              </ValidationProvider>

              <ValidationProvider
                name="Auth-Amount"
                rules="required"
                v-slot="{ errors }"
              >
                <label>{{ fields[0] }} </label>
                <input type="number" v-model="authorizedAmount" />
                <span>{{ errors[0] }}</span>
              </ValidationProvider>

              <ValidationProvider
                name="Issued-Amount"
                rules="required"
                v-slot="{ errors }"
              >
                <label>{{ fields[1] }} </label>
                <input type="number" v-model="issuedAmount" />
                <span>{{ errors[0] }}</span>
              </ValidationProvider>

              <ValidationProvider
                name="Auth-Capital"
                rules="required"
                v-slot="{ errors }"
              >
                <label>{{ fields[2] }} </label>
                <input type="number" v-model="authorizedCapital" />
                <span>{{ errors[0] }}</span>
              </ValidationProvider>

              <ValidationProvider
                name="Issued-Capital"
                rules="required"
                v-slot="{ errors }"
              >
                <label>{{ fields[3] }} </label>
                <input type="number" v-model="issuedCapital" />
                <span>{{ errors[0] }}</span>
              </ValidationProvider>

              <button type="submit" @click="onSubmit" class="btn">
                Add Security Class
              </button>
            </form>
          </ValidationObserver>

          <button class="btn close-btn" v-on:click="$emit('close')">
            Close
          </button>
        </div>
      </div>
    </div>
  </transition>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";

import { ValidationProvider, ValidationObserver, extend } from "vee-validate";
import { required } from "vee-validate/dist/rules";
import { v4 as uuidv4 } from "uuid";
import { TableData } from "@/types/types";

extend("required", {
  ...required,
  message: "This field is required",
});

@Component({
  components: {
    ValidationProvider,
    ValidationObserver,
  },
  name: "Modal",
})
export default class Modal extends Vue {
  nameSelected = "";
  authorizedAmount = "";
  issuedAmount = "";
  authorizedCapital = "";
  issuedCapital = "";

  options = {
    series: [
      { value: "series-f", text: "Series F" },
      { value: "series-g", text: "Series G" },
      { value: "series-h", text: "Series H" },
      { value: "series-i", text: "Series I" },
      { value: "series-j", text: "Series J" },
    ],
  };

  fields = [
    "Authorized Amount",
    "Issued Amount",
    "Authorized Capital",
    "Issued Capital",
  ];

  onSubmit() {
    const newSecurityClass: TableData = {
      id: uuidv4(),
      name: this.nameSelected,
      nominalValue: 7,
      authorizedAmount: Number(this.authorizedAmount),
      issuedAmount: Number(this.issuedAmount),
      authorizedCapital: Number(this.authorizedCapital),
      issuedCapital: Number(this.issuedCapital),
    };
    const areAllPopulated = Object.values(newSecurityClass).every(
      (item) => item
    );
    areAllPopulated &&
      this.$emit("test", newSecurityClass) &&
      this.$emit("close");
  }
}
</script>

<style lang="scss">
$green: #50a53e;
$white: #ffffff;

.btn {
  box-sizing: border-box;
  background-color: transparent;
  border: 2px solid $green;
  border-radius: 0.6em;
  color: $green;
  cursor: pointer;
  font-size: 0.7rem;
  font-weight: 400;
  line-height: 1;
  margin: 20px;
  padding: 1.2em 2.8em;
  text-decoration: none;
  text-align: center;
  text-transform: uppercase;
  font-weight: 700;
  transition: box-shadow 200ms ease-in-out, color 200ms ease-in-out;

  &:hover,
  &:focus {
    outline: 0;
  }

  &:hover {
    color: $white;
    box-shadow: 0 0 40px 40px $green inset;
  }
}

.close-btn {
  padding: 15px;
}

form {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  display: flex;
  flex-direction: column;
  align-items: center;
  label {
    display: block;
    margin-block-start: 20px;
  }

  span {
    display: block;
  }

  select,
  input {
    width: 200px;
    padding: 0.5rem;
  }
}

.layer {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 9998;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.4s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  width: 40vw;
  height: 80vh;
  margin: 0px auto;
  padding: 20px 30px;
  min-height: 35px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.2s ease;
  font-family: Helvetica, Arial, sans-serif;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.modal-enter {
  opacity: 0;
}

.modal-leave-active {
  opacity: 0;
}

.modal-enter .modal-container,
.modal-leave-active .modal-container {
  -webkit-transform: scale(0.95);
  transform: scale(0.95);
}
</style>

<template>
  <transition name="modal">
    <div class="layer" v-on:click="$emit('close')">
      <div class="modal-wrapper">
        <div class="modal-container">
          <slot name="header">Header</slot>
          <button class="modal-default-button" v-on:click="$emit('close')">
            Close
          </button>
        </div>
      </div>
    </div>
  </transition>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

@Component({
  name: "Modal",
})
export default class Modal extends Vue {
  @Prop({ required: true }) show!: Boolean;
}
</script>

<style lang="scss">

$green: #50a53e;
$red: #ef524f;
$blue: #3498db;
$orange: #f5ab15;
$purple: #ba64c8;
$dark-grey: #535a74;
$white: #ffffff;


.btn {
  box-sizing: border-box;
  background-color: transparent;
  border: 2px solid $green;
  border-radius: 0.6em;
  color: $green;
  cursor: pointer;
  font-size: 1rem;
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
  width: 300px;
  margin: 0px auto;
  padding: 20px 30px;
  min-height: 35px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.2s ease;
  font-family: Helvetica, Arial, sans-serif;
}

.modal-default-button {
  float: right;
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

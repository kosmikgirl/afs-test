<template>
  <div class="transfer-row">
    <div>
      <p class="transfer-row__state">{{ transfer.state }}</p>
    </div>
    <div class="details-container">
      <div>
        <label>Date</label>
        <p>{{ transfer.recordDate }}</p>
      </div>
      <div>
        <label>Type</label>
        <p>{{ transfer.type }}</p>
      </div>
      <div>
        <label>Amount</label>
        <p>{{ transfer.amount }}</p>
      </div>
      <span v-if="transfer.forgottenProperty">{{
        transfer.forgottenProperty
      }}</span>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import { Component, Prop } from "vue-property-decorator";
import { Transaction } from "@/types/types";

@Component({
  name: "TransferRow",
})
export default class TransferRow extends Vue {
  @Prop({ required: true }) transfer!: Transaction;
}
</script>

<style lang="scss">
.transfer-row {
  margin: 1rem;
  padding: 1rem;
  max-width: 500px;
  background-color: white;
  border: none;
  border-radius: 10px;
  display: flex;
  justify-content: space-around;
  cursor: pointer;

  display: flex;
  flex-direction: column;
  align-items: center;

  p,
  label {
    padding-block: 10px;

  }

  label {
    color: lightgray
  }

  p {
    color: gray;
  }

  &__state {
    font-size: 1.5rem;
    width: fit-content;
    display: block;
    margin-bottom: 30px;
  }
}

.details-container {
  display: flex;
  justify-content: space-around;
  width: 100%;
}
</style>

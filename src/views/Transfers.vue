<template>
  <div class="transfers">
    <h1 class="title is-1">Transfers</h1>
    <label
      >Search
      <input type="date" v-model="searchTerms" />
    </label>
    <div>
      <button class="btn edit-btn" @click.once="updateTransfers">
        Update transfers
      </button>
      <div class="search-container">
        <div v-if="searchedTransfers">
          <transfer-row
            :key="transfer.transactionIdentifier"
            v-for="transfer in searchedTransfers"
            :transfer="transfer"
            :class="[arrayOfColors[findRandomColor()]]"
          />
        </div>
        <p v-if="searchedTransfers.length === 0">
          No results to show &#128546;
        </p>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import { Transaction } from "@/types/types";
import TransferRow from "@/components/transferRow.vue";
import transfers from "@/assets/data";
@Component({
  name: "Transfers",
  components: { TransferRow },
})
export default class Transfers extends Vue {
  searchTerms = "";
  transfers = transfers;
  arrayOfColors = ["purple", "green", "orange", "red"];
  // eslint-disable-next-line @typescript-eslint/explicit-module-boundary-types
  get searchedTransfers() {
    if (this.searchTerms) {
      // custom search, should be improved upon
      const searchArray: Transaction[] = [];
      this.transfers.forEach((transfer: Transaction) => {
        transfer.recordDate?.includes(this.searchTerms) &&
          searchArray.push(transfer);
      });
      return searchArray;
    }
    return this.transfers;
  }

  findRandomColor() {
    return (
      Math.floor(Math.random() * transfers.length) % this.arrayOfColors.length
    );
  }

  updateTransfers(): void {
    const newItem: Transaction = {
      splitFactor: null,
      exDate: null,
      amount: 10000,
      companyId: "568fa387-43d1-499a-bba2-25089f5a881a",
      notes: null,
      pricePerShare: null,
      recordDate: "2021-07-01",
      securityClassId: "ab983cfe-a932-4e25-98ea-f5928a839fe1",
      securityClass: { name: "Common" },
      state: "OLD",
      toSecurityHolderId: "dd971e7f-386b-45dd-93e1-666fbeed0a55",
      toSecurityHolder: {
        fullName: "Jeff Dunlap",
        type: "PERSON",
      },
      transactionIdentifier: "41095fdb-6b52-4257-aef8-dc523d782e53",
      positionWithinDay: 3,
      type: "ISSUE_STOCK",
    };

    const copyTransfers = [newItem, ...this.transfers];

    const arr = this.transfers.map(
      (transfer) =>
        copyTransfers.find(
          (el) => el.transactionIdentifier === transfer.transactionIdentifier
        ) || transfer
    );

    arr.forEach((transfer) => {
      const forgottenProperty = `Important data: ${(Math.random() * 100000000)
        .toString()
        .slice(1, 8)}`;
      Vue.set(transfer, "forgottenProperty", forgottenProperty);
    });
    this.transfers = arr;
  }
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

.search-container {
  background-color: $dark-grey;
  padding: 50px;
  color: $white;
  font-weight: bold;

  @media only screen and (max-width: 600px) {
    padding-inline: 0;
  }
}

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

.edit-btn {
  margin: 2rem;
}

.purple {
  border-left: 10px solid $purple;
}

.green {
  border-left: 10px solid $green;
}
.orange {
  border-left: 10px solid $orange;
}

.red {
  border-left: 10px solid $red;
}

.transfer-row__state {
  border: none;
}
</style>

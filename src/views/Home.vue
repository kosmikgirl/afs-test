<template>
  <div class="home">
    <h1>This is a table with some important data</h1>

    <button id="show-modal" class="btn edit-btn" v-on:click="showModal = true">Add security class</button>

    <modal v-if="showModal" @test="addSecurityClass" v-on:close="showModal = false">
      <span slot="header">New header</span>
    </modal>
    <b-table :data="tableDataToShow" :columns="columns"></b-table>
  </div>
</template>

<script lang="ts" setup>
/* eslint-disable @typescript-eslint/no-explicit-any */
import { Component, Vue, Watch } from "vue-property-decorator";
import { TableData } from "@/types/types";
import Modal from "./Modal.vue";

@Component({
  components: { Modal },
})
export default class Home extends Vue {
  tableData: TableData[] = [];
  tableDataToShow: TableData[] = [];
  columns = [
    {
      label: "Security class",
      field: "name",
    },
    {
      label: "Authorized amount",
      field: "authorizedAmount",
    },
    {
      label: "Issued amount",
      field: "issuedAmount",
    },
    {
      label: "Authorized Capital",
      field: "authorizedCapital",
    },
    {
      label: "Issued capital",
      field: "issuedCapital",
    },
  ];
  loading = false;
  showModal = false;


  @Watch('tableData')
  public addDataToTable(data: any) {
    const keys = Object.keys(data[0]);
      // todo change type
      let allElements: any = {};
      keys.forEach((key) => {
        const sum = data.reduce(
          (accum: any, item: any) => accum + Number(item[key as keyof TableData]),
          0
        );
        key === "name"
          ? (allElements[key] = "Total")
          : (allElements[key] = sum);
      });
      const newTableData = [...this.tableData, allElements]
      this.tableDataToShow = newTableData;
  }


  addSecurityClass(newClass: TableData) {
    this.tableData.push(newClass);
  }

  // mounted works fine if your ide complains about it
  // eslint-disable-next-line @typescript-eslint/explicit-module-boundary-types
  async mounted() {
    try {
      const data = await this.getData();
      this.tableData = data;
      this.loading = false;
    } catch (error) {
      console.error(error);
    }
  }


  async getData(): Promise<TableData[]> {
    return [
      {
        id: "42f2462d-49d0-4e91-8fe1-de2e656b0f06",
        name: "Series A",
        nominalValue: 5,
        authorizedAmount: 1500,
        issuedAmount: 500,
        authorizedCapital: 7550,
        issuedCapital: 2500,
      },
      {
        id: "42f2462d-49d0-4e91-8fe1-de2e656b0f06",
        name: "Series B",
        nominalValue: 10,
        authorizedAmount: 15000,
        issuedAmount: 5000,
        authorizedCapital: 150000,
        issuedCapital: 50000,
      },
      {
        id: "fd78c11b-e3d2-455a-99b0-49907a75c463",
        name: "Series C",
        nominalValue: 1,
        authorizedAmount: 96876,
        issuedAmount: 61760,
        authorizedCapital: 96876,
        issuedCapital: 61760,
      },
      {
        id: "d8654cb0-8986-4fbc-b969-025e514cb934",
        name: "Series D",
        nominalValue: 1,
        authorizedAmount: 10110,
        issuedAmount: 1100,
        authorizedCapital: 10110,
        issuedCapital: 1100,
      },
    ];
  }
}
</script>

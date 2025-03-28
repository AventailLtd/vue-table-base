<template>
  <div>
    <table-base :items="getItems()" :fields="fields" :order-by="orderBy" row-clickable @orderChanged="orderChanged" @rowClicked="onRowClicked">
      <template #sortIconAsc>
        <small>
          (active asc)
        </small>
      </template>
      <template #sortIconDesc>
        <small>
          (active desc)
        </small>
      </template>
      <template #sortIconClassNotActive>
        <small>
          (orderable)
        </small>
      </template>
      <template #thInner="data">
        <span v-if="data.field.tooltip !== undefined" :title="data.field.tooltip">
          {{ data.field.label }} (with tooltip)
        </span>
        <span v-else>
          {{ data.field.label }}
        </span>
      </template>
      <template #td="data">
        <div v-if="data.field === 'price'">
          {{ data.value }} + Using another column value: {{ data.row.name }}
        </div>
        <div v-else>
          {{ data.value}}
        </div>
      </template>
      <template #row-details>
        <div class="row-details">
          Row details
          <textarea class="w-100"/>
        </div>
      </template>
    </table-base>
    <div>
      Order:
      {{ orderBy }} {{ !sortDesc ? 'ASC' : 'DESC' }}
    </div>
    <div>
      ClickedRow: {{ clickedRow }}
    </div>
  </div>
</template>

<script>

import {defineComponent} from "vue";
import TableBase from "@/components/table-base/TableBase.vue";

export default defineComponent({
  components: {
    TableBase,
  },
  data() {
    return {
      fields: [
        {
          key: 'id',
          label: 'ID',
        },
        {
          key: 'name',
          label: 'Name',
          tooltip: 'Tooltip Test',
        },
        {
          key: 'price',
          label: 'Price',
          sort: false,
        },
      ],
      items: [
        {
          id: 1,
          name: 'Test1',
          price: 150,
          trClass: 'bg-danger',
        },
        {
          id: 2,
          name: 'Test2',
          price: 250,
          tdClass: {
            price: 'bg-success',
          }
        },
        {
          id: 3,
          name: 'Test3',
          price: 210,
          trClass: 'bg-warning',
        },
        {
          id: 4,
          name: 'Test4',
          price: 22,
        },
      ],
      orderBy: 'name',
      sortDesc: false,
      clickedRow: null,
    }
  },
  methods: {
    orderChanged({ sortDesc, orderBy }) {
      this.sortDesc = sortDesc
      this.orderBy = orderBy
    },

    onRowClicked(item) {
      this.clickedRow = item
    },

    getItems() {
      // add row details extra row after the first row
      return this.items.map(item => {
        item._showRowDetails = item.id === 1;
        return item
      })
    },
  }
})
</script>


<style>
.bg-danger {
  background-color: #da6c6c;
}
.bg-success {
  background-color: #2ba41e;
}
.bg-warning {
  background-color: #f0ad4e;
}
.w-100 {
  width: 100%;
}
</style>

<template>
  <div class="column bg-light-green q-pb-md">
    <div class="row justify-center">
      <span class="text-h5 color-dark text-weight-bolder q-my-lg"
        >Controle de Gastos</span
      >
    </div>

    <div class="row bg-light-green justify-around">
      <div class="card bg-white col-3 q-pa-md">
        <span class="text-h6 text-info"
          >Entradas <q-icon name="arrow_upward" />
        </span>
        <p class="income-display text-info">
          R$ {{ income.toLocaleString("pt-BR", { minimumFractionDigits: 2 }) }}
        </p>
      </div>

      <div class="card bg-white col-3 q-pa-md">
        <span class="text-h6 text-info"
          >Saídas <q-icon name="arrow_downward"></q-icon>
        </span>
        <p class="expense-display text-info bg-">
          R$ {{ expense.toLocaleString("pt-BR", { minimumFractionDigits: 2 }) }}
        </p>
      </div>

      <div class="card total col-3 bg-blue-grey-4 q-pa-md">
        <span class="text-h6 text-white text-weight-bolder">
          Total <q-icon name="attach_money" />
        </span>
        <p class="total-display text-white">
          R$ {{ total.toLocaleString("pt-BR", { minimumFractionDigits: 2 }) }}
        </p>
      </div>
    </div>
  </div>
  <div class="q-pa-md">
    <q-btn
      outline
      color="blue-grey"
      label="+Nova Transação"
      class="border"
      @click="newTransaction"
    >
    </q-btn>
  </div>

  <q-dialog v-model="openModal">
    <q-card class="card-width">
      <span>Nova Transação</span>
      <q-card-section>
        <q-input
          rounded
          outlined
          v-model="name"
          label="Descrição"
          color="dark"
        ></q-input>
        <q-input
          v-bind:value="'R$ ' + { minimumFractionDigits: 2 }"
          v-model.number="value"
          type="number"
          style="max-width: 200px"
          color="black"
          label="Valor"
        ></q-input>
        <q-input
          square
          filled
          v-model="date"
          type="date"
          color="black"
          label="Data"
        >
        </q-input>
        <div class="justify-around">
          <q-btn color="red" label="Cancelar" @click="cancel()"></q-btn>

          <q-btn color="green" label="Salvar" @click="save()"></q-btn>
        </div>
      </q-card-section>
    </q-card>
  </q-dialog>

  <div class="q-pa-md"></div>
  <div class="row q-col-gutter-sm"></div>
  <div class="col"></div>
  <q-table :rows="rows" :columns="columns">
    <template v-slot:body-cell-actions="row">
      <q-td :row="row">
        <q-btn
          dense
          round
          flat
          color="grey"
          @click="deleteRow(row)"
          icon="delete"
        />
      </q-td>
    </template>
  </q-table>
</template>

<script setup>
import { ref } from "vue";
const income = ref(0);
const expense = ref(0);
const total = ref(0);
const name = ref("");
const value = ref(0);
const date = ref("");
const openModal = ref(false);
const rows = ref([]);
const columns = [
  {
    label: "Descrição",
    align: "left",
    field: "name",
  },
  {
    label: "Valor",
    align: "center",
    field: "value",
  },
  {
    label: "Data",
    align: "center",
    field: "date",
  },
  {
    name: "actions",
    align: "center",
  },
];

function newTransaction() {
  openModal.value = true;
}

function cancel() {
  openModal.value = false;
}

function save() {
  const newTransaction = {
    name: name.value,
    value: value.value,
    date: date.value,
  };
  rows.value.push(newTransaction);
  openModal.value = false;

  if (newTransaction.value > 0) {
    income.value += newTransaction.value;
  } else {
    expense.value += newTransaction.value;
  }
  total.value += newTransaction.value;
}
function deleteRow(row) {
  const index = row.rowIndex;
  const value = rows.value[index].value;

  rows.value.splice(index, 1);

  if (value > 0) {
    income.value -= value;
  } else {
    expense.value -= value;
  }
  total.value -= value;
}
</script>

<style scoped>
.card {
  border-radius: 0.25rem;
}
</style>

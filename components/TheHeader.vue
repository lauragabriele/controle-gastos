<template>
  <div class="column bg-light-green q-pb-md">
    <div class="row justify-center">
      <span class="text-h5 color-dark text-weight-bolder q-my-lg"
        >Controle de Gastos</span
      >
    </div>

    <div class="row bg-light-green justify-around">
      <div class="card bg-white col-3">
        <span class="text-h6 text-info" color="info"
          >Entradas <q-icon name="arrow_upward" />
        </span>
        <p class="incomeDisplay text-info">R$ {{ income.toFixed(2) }}</p>
      </div>

      <div class="card bg-white col-3">
        <span class="text-h6 text-info"
          >Saídas <q-icon name="arrow_downward"></q-icon>
        </span>
        <p class="expenseDisplay text-info bg-">R$ {{ expense.toFixed(2) }}</p>
      </div>

      <div class="card total col-3 bg-blue-grey-4">
        <span class="text-h6 text-white text-weight-bolder">
          Total <q-icon name="attach_money"></q-icon>
        </span>
        <p class="totalDisplay text-white">R$ {{ total.toFixed(2) }}</p>
      </div>
    </div>
  </div>

  <div>
    <div class="q-app" style="min-height: 100vh">
      <div class="q-pa-md"></div>
      <q-btn
        outline
        color="blue-grey"
        label="+Nova Transação"
        class="border"
        @click="newTransaction"
      >
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
                <template v-slot:prepend>
                  <q-icon name="event"></q-icon>
                </template>
              </q-input>
              <div class="justify-around">
                <q-btn color="red" @click="cancel()"> Cancelar </q-btn>

                <q-btn color="green" @click="save()">Salvar</q-btn>
              </div>
            </q-card-section>
          </q-card>

          <div></div>
        </q-dialog>
      </q-btn>
          
            <q-table

        :rows="rows"
        :columns="columns"
  ></q-table>     
    </div>
  </div>
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
    label: 'Descrição',
    align: 'left',
    field:  'name'
  },
  {
    label: 'Valor',
    align: 'center',
    field: 'value',
  },
  {
    label: 'Data',
    align: 'center',
    field: 'date',
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

</script>

<style scoped>
.card {
  padding: 1.5rem 2rem;
  border-radius: 0.25rem;
}
.card-width {
  width: 20vw;
}
</style>

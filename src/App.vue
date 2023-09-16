<script>
import { defineComponent, ref, onMounted } from "vue";
export default defineComponent({
  setup() {
    const conta = ref({
      descricao: "",
      valor: "",
      vencimento: "",
    });

    const contas = ref([]);

    const storage = {
      setContas(contasArray) {
        localStorage.setItem("contas", JSON.stringify(contasArray));
      },
      getContas() {
        return JSON.parse(localStorage.getItem("contas"));
      },
    };

    const cadastrar = () => {
      if (
        conta.value?.descricao?.length < 1 ||
        conta.value?.valor?.length < 1 ||
        conta.value?.vencimento?.length < 1
      ) {
        return alert("preencha todos os campos!");
      }

      contas.value.push({
        id: Date.now(),
        ...conta.value,
        situacao: "AB",
      });

      storage.setContas(contas.value);

      conta.value = {
        descricao: "",
        valor: "",
        vencimento: "",
      };
    };

    const apagar = (id) => {
      contas.value = contas.value.filter((conta) => conta.id !== id);
      storage.setContas(contas.value);
    };

    onMounted(() => {
      contas.value = storage.getContas();
    });

    return {
      contas,
      cadastrar,
      conta,
      apagar,
    };
  },
});
</script>

<template>
  <div class="container mt-5">
    <h1 class="text-center">Contas a Pagar</h1>
    <hr />
    <div class="form">
      <h4>Cadastrar nova conta</h4>
      <div class="row">
        <div class="col-12 mb-2">
          <input
            v-model="conta.descricao"
            type="text"
            class="form-control"
            placeholder="descrição"
          />
        </div>
        <div class="col-6 mb-2">
          <input
            v-model="conta.valor"
            type="number"
            step="0.01"
            class="form-control"
            placeholder="valor"
          />
        </div>
        <div class="col-6 mb-2">
          <input
            v-model="conta.vencimento"
            type="date"
            class="form-control"
            placeholder="vencimento"
          />
        </div>
        <div class="col-12" style="display: flex; justify-content: flex-end">
          <button @click="cadastrar" class="btn btn-primary">Gravar</button>
        </div>
      </div>
    </div>
    <hr />
    <table class="table">
      <thead>
        <tr>
          <th>#</th>
          <th>Descrição</th>
          <th>Valor</th>
          <th>Vencimento</th>
          <th>Situação</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="conta in contas" :key="conta.id">
          <td>{{ conta.id }}</td>
          <td>{{ conta.descricao }}</td>
          <td>{{ conta.valor }}</td>
          <td>{{ conta.vencimento }}</td>
          <td>{{ conta.situacao }}</td>
          <td>
            <button class="btn btn-sm btn-primary" style="margin-right: 4px">E</button>
            <button @click="apagar(conta.id)" class="btn btn-sm btn-danger">A</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import { defineComponent, ref } from 'vue'
export default defineComponent({
  setup() {
    const conta = ref({
      descricao: '',
      valor: '',
      vencimento: ''
    })

    const contas = ref([
      {
        id: 1,
        descricao: 'academia',
        valor: 100,
        vencimento: '2023-09-15',
        situacao: 'AB'
      },
      {
        id: 2,
        descricao: 'energia',
        valor: 180,
        vencimento: '2023-09-25',
        situacao: 'AB'
      }
    ])

    const cadastrar = () => {
      if (
        conta.value?.descricao?.length < 1 ||
        conta.value?.valor?.length < 1 ||
        conta.value?.vencimento?.length < 1
      ) {
        return alert('preencha todos os campos!')
      }

      contas.value.push({
        id: Date.now(),
        ...conta.value,
        situacao: 'AB'
      })

      conta.value = {
        descricao: '',
        valor: '',
        vencimento: ''
      }
    }

    return {
      contas,
      cadastrar,
      conta
    }
  }
})
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
          <input v-model="conta.valor" type="text" class="form-control" placeholder="valor" />
        </div>
        <div class="col-6 mb-2">
          <input
            v-model="conta.vencimento"
            type="text"
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
            <button class="btn btn-sm btn-danger">A</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

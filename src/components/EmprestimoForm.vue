<template>
  <div class="container">
      <div class="form-wrapper">
        <h1 class="form-title">Solicitação de Empréstimo</h1>
        <form @submit.prevent="submitForm">
      <div class="mb-3">
        <label for="creditValue" class="form-label">Valor do Crédito:</label>
        <input type="number" id="creditValue" v-model.number="creditValue" class="form-control" required>
      </div>

      <div class="mb-3">
        <label for="loanType" class="form-label">Tipo de Empréstimo:</label>
        <select id="loanType" v-model="loanType" class="form-select" required>
          <option value="pessoaFisica">Pessoa Física</option>
          <option value="pessoaJuridica">Pessoa Jurídica</option>
        </select>
      </div>

      <div class="mb-3">
        <label for="numberOfInstallments" class="form-label">Quantidade de Parcelas:</label>
        <input type="number" id="numberOfInstallments" v-model="numberOfInstallments" min="5" max="72" class="form-control" required>
      </div>

      <div class="mb-3">
        <label for="firstDueDate" class="form-label">Data do Primeiro Vencimento:</label>
        <input type="date" id="firstDueDate" v-model="firstDueDate" class="form-control" required>
      </div>

      <button type="submit" class="btn btn-primary">Enviar</button>
    
    </form>

    <div v-if="result">
      <h3>Resultado da Solicitação</h3>
      <p>Status do Crédito: {{ result.status }}</p>
      <p>Valor Total com Juros: {{ result.totalWithInterest }}</p>
    </div>
    
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import 'bootstrap/dist/css/bootstrap.min.css';
import 'bootstrap/dist/js/bootstrap.bundle.min.js';


export default {
  data() {
    return {
      creditValue: null,
      loanType: null,
      numberOfInstallments: null,
      firstDueDate: null,
      result: null
    };
  },
  methods: {
    async submitForm() {
      // Validação do valor do crédito
      if (this.creditValue <= 0 || this.creditValue > 1000000) {
        alert('O valor do crédito deve estar entre 0 e 1 milhão de reais.');
        return;
      }

      // Validação da data do primeiro vencimento
      const firstDueDate = new Date(this.firstDueDate);
      const minDate = new Date();
      minDate.setDate(minDate.getDate() + 15); // 15 dias a partir da data atual
      const maxDate = new Date();
      maxDate.setDate(maxDate.getDate() + 40); // 40 dias a partir da data atual

      if (firstDueDate < minDate || firstDueDate > maxDate) {
        alert('A data do primeiro vencimento deve ser no mínimo 15 dias e no máximo 40 dias a partir da data atual.');
        return;
      }

      // Fazer requisição para a API
      try {
        const response = await axios.post('URL_DA_API', {
          creditValue: this.creditValue,
          loanType: this.loanType,
          numberOfInstallments: this.numberOfInstallments,
          firstDueDate: this.firstDueDate,
          // Enviar outros campos para a API
        });
        this.result = response.data;
      } catch (error) {
        console.error('Erro ao enviar requisição:', error);
      }
    }
  }
};
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.form-wrapper {
  border: 1px solid hsl(180, 33%, 1%);
  background-color: #e7f3e7; /* Fundo verde claro */
  padding: 20px;
  border-radius: 10px;
  width: 400px;
  margin-top: -100px;
}

.form-title {
  text-align: center;
}

</style>

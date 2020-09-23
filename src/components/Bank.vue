<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      Por favor, digite os dados da sua agência e conta abaixo e escolha a operação desejada:
    </p>
  </div>
  <div class="form-group container">
      <label for="branch">Agência: </label>
      <input v-model="account.branch" placeholder="Agência" type="number" id="branch"
                    class="form-control">
      <label for="account">Conta: </label>
      <input v-model="account.account" placeholder="Conta" type="number" id="account"
                    class="form-control">
    <br>
    <input type="button" value="Ver saldo" v-on:click="showBalance"/>
    <input type="button" value="Realizar depósito" v-on:click="showAmountInputDeposit"/>
    <input type="button" value="Realizar saque" v-on:click="showAmountInputWithdraw"/>
    <div v-if="deposit.showAmountInput">
      <br>
      <label for="deposit_amount">Valor para depósito: </label>
      <input v-model="deposit.amount" placeholder="Valor" type="number" id="deposit_amount"
                    class="form-control">
      <br>
      <input type="button" value="Depositar" v-on:click="doDeposit"/>
      <p v-if="deposit.message != null">{{deposit.message}}</p>
    </div>
    <div v-if="withdraw.showAmountInput">
      <br>
      <label for="deposit_amount">Valor para saque: </label>
      <input v-model="withdraw.amount" placeholder="Valor" type="number" id="withdraw_amount"
                    class="form-control">
      <br>
      <input type="button" value="Sacar" v-on:click="doWithdraw"/>
      <p v-if="withdraw.message != null">{{withdraw.message}}</p>
    </div>
    <div v-if="account.balance != null">
      <br>
      <p>Saldo da conta: R${{account.balance}}</p>
    </div>
    <div v-if="account.message != null">
      <br>
      <p>{{account.message}}</p>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'HelloWorld',
    props: {
      msg: String
    },
    methods: {
      showAmountInputDeposit() {
        /** Mostra o input para inserir o valor a ser depositado */
        this.deposit.showAmountInput = true;
        this.withdraw.showAmountInput = false;
      },
      showAmountInputWithdraw() {
        /** Mostra o input para inserir o valor a ser sacado */
        this.withdraw.showAmountInput = true;
        this.deposit.showAmountInput = false;
      },
      doDeposit() {
        /** Realiza o depósito */
        if(this.deposit.amount === 0){
          this.deposit.message = 'Valor precisa ser maior que 0';
          return;
        }
        if(this.account.account === '' || this.account.branch === ''){
          this.deposit.message = 'Por favor, preencha os dados corretamanete.';
          return;
        }
        var data = {
          account: this.account.account,
          branch: this.account.branch,
          amount: this.deposit.amount
        };
        fetch("http://127.0.0.1:8000/api/currentaccount/withdrawdeposit", {
          "method": "POST",
          body: JSON.stringify(data),
          credentials: "same-origin",
        })
          .then(response => {
            console.log(response);
            if (response.ok) {
              return response.json()
            } else {
              alert("Erro interno. Por favor, confira a agência e conta e tente novamente");
            }
          })
          .then(response => {
            this.deposit.message = response;
            this.account.balance = null;
          })
          .catch(err => {
            console.log(err);
            alert("Erro interno. Por favor, confira a agência e conta e tente novamente");
          });
      },
      doWithdraw() {
        /** Realiza o saque */
        if(this.withdraw.amount === 0){
          this.withdraw.message = 'Valor precisa ser maior que 0';
          return;
        }
        if(this.account.account === '' || this.account.branch === ''){
          this.withdraw.message = 'Por favor, preencha os dados corretamanete.';
          return;
        }
        var data = {
          account: this.account.account,
          branch: this.account.branch,
          amount: this.withdraw.amount * -1
        };
        fetch("http://127.0.0.1:8000/api/currentaccount/withdrawdeposit", {
          "method": "POST",
          body: JSON.stringify(data),
          credentials: "same-origin",
        })
          .then(response => {
            console.log(response);
            if (response.ok) {
              return response.json()
            } else {
              alert("Erro interno. Por favor, confira a agência e conta e tente novamente");
            }
          })
          .then(response => {
            this.withdraw.message = response;
            this.account.balance = null;
          })
          .catch(err => {
            console.log(err);
            alert("Erro interno. Por favor, confira a agência e conta e tente novamente");
          });
      },
      showBalance() {
        if(this.account.account === '' || this.account.branch === ''){
          this.account.balance = null;
          this.account.message = 'Por favor, preencha os dados corretamanete.';
          return;
        }
        /** Retorna o saldo da conta */
        fetch("http://127.0.0.1:8000/api/currentaccount/balance?account=" + this.account.account + "&branch=" + this.account.branch, {
          "method": "GET",
          credentials: "same-origin",
        })
          .then(response => {
            console.log(response);
            if (response.ok) {
              return response.json()
            } else {
              alert("Erro interno");
            }
          })
          .then(response => {
            console.log(response);
            this.account.balance = response;
            this.account.message = null;
            this.deposit.showAmountInput = false;
            this.withdraw.showAmountInput = false;
          })
          .catch(err => {
            console.log(err);
            alert("Erro interno. Por favor, confira a agência e conta e tente novamente");
          });
      }
    },
    data() {
      return {
        account: {
          branch: '',
          account: '',
          balance: null,
          message: null
        },
        deposit: {
          amount: 0,
          showAmountInput: false,
          message: null
        },
        withdraw: {
          amount: 0,
          showAmountInput: false
        }
      }
    }
  }
</script>

<style scoped>
  h3 {
    margin: 40px 0 0;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    margin: 0 10px;
  }

  a {
    color: #42b983;
  }
</style>

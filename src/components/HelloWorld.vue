<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      Por favor, digite os dados da sua agência e conta abaixo e escolha a operação desejada;
    </p>
  </div>
  <div class="form-group">
    <label for="branch">Agência: </label>
    <input v-model="account.branch" placeholder="Agência" type="number" id="branch" class="form-control">
    <br>
    <label for="account">Conta: </label>
    <input v-model="account.account" placeholder="Conta" type="number" id="account" class="form-control">
    <br>
    <input type="button" value="Ver saldo" v-on:click="showBalance"/>
    <input type="button" value="Realizar depósito" v-on:click="showAmountInputDeposit"/>
    <input type="button" value="Realizar saque" v-on:click="showBalance"/>
    <div v-if="deposit.showAmountInput">
      <label for="deposit_amount">Valor para depósito: </label>
      <input v-model="deposit.amount" placeholder="Valor" type="number" id="deposit_amount" class="form-control">
      <input type="button" value="Depositar" v-on:click="doDeposit"/>
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
        this.deposit.showAmountInput = true;
      },
      doDeposit() {
        var data = {
          account: this.account.account,
          branch: this.account.branch,
          amount: this.deposit.amount
        };
        fetch("http://127.0.0.1:8000/api/currentaccount/withdrawdeposit", {
          "method": "POST",
          body: JSON.stringify(data),
          mode: 'no-cors'
        })
          .then(response => {
            console.log(response);
            // if (response.ok) {
            //   return response.json()
            // } else {
            //   alert("Server returned " + response.status + " : " + response.statusText);
            // }
          })
          .then(response => {
            console.log(response.body);
          })
          .catch(err => {
            console.log(err);
          });
      },
      showBalance() {
        fetch("http://127.0.0.1:8000/api/currentaccount/balance?account="+this.account.account+"&branch="+this.account.branch, {
          "method": "GET",
          credentials: "same-origin",
        })
          .then(response => {
            console.log(response);
            if (response.ok) {
              return response.json()
            } else {
              alert("Server returned " + response.status + " : " + response.statusText);
            }
          })
          .then(response => {
            console.log(response);
          })
          .catch(err => {
            console.log(err);
          });
      }
    },
    data() {
      return {
        account: {
          branch: '',
          account: ''
        },
        deposit: {
          amount: 0,
          showAmountInput: false
        }
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
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

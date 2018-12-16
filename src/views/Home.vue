<template>
  <div class="home">
    <table class="table">
      <tr>
        <th>Address</th>
        <th>Balance</th>
        <th>Transaction</th>
      </tr>
      <tr v-bind:key="index" v-for="(item, index) in accounts">
        <td>{{item}}</td>
        <td>{{balance[index]}}</td>
        <td>
          <section>
            <button class="button" @click="setModal(item)">Make Transaction</button>

            <b-modal :active.sync="isComponentModalActive" has-modal-card>
              <modal-form :amount="amount" :fromAddress="fromAddress" :accounts="accounts"></modal-form>
            </b-modal>
          </section>
        </td>
      </tr>
    </table>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import Web3 from "web3";
// @ts-ignore
import ModalForm from "@/components/ModalForm";
import Component from "vue-class-component";

@Component({
  components: {
    ModalForm
  }
})
export default class Home extends Vue {
  isComponentModalActive: boolean = false;
  amount: number = null;
  fromAddress: string = ''
  accounts: Array<{string}> = []
  balance: Array<{number}> = []
  

  setModal(item) {
    this.fromAddress = item;
    this.isComponentModalActive = true;
  }

  getAccountBalance() {
    const web3 = new Web3(
      new Web3.providers.HttpProvider("http://127.0.0.1:7545/")
    );
    const accounts = web3.eth.getAccounts((err, acc) => {
      // @ts-ignore
      this.accounts = acc;
      const balanceArray = this.balance;
      for (let i = 0; i < acc.length; i++) {
        const weiBalance = web3.eth.getBalance(acc[i]);
        balanceArray.push(
          // @ts-ignore
          web3.fromWei(weiBalance.toNumber(), "ether") + " ETH"
        );
      }
    });
  }

  mounted (){
      this.getAccountBalance()
  }
}
</script>

<style>
.table {
  margin-left: auto;
  margin-right: auto;
}
</style>
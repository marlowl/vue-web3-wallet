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
import { Vue, Component, Prop } from "vue-property-decorator";
import Web3 from "web3";
import ModalForm from "@/components/ModalForm.vue";

@Component({
  components: {
    ModalForm
  }
})
export default class Home extends Vue {
  isComponentModalActive: boolean = false;
  amount: number = null;
  fromAddress: string = "";
  accounts: string[] = [];
  balance: string[] = [];

  setModal(item) {
    this.fromAddress = item;
    this.isComponentModalActive = true;
  }

  getAccountBalance() {
    const web3 = new Web3(
      new Web3.providers.HttpProvider("http://127.0.0.1:7545/")
    );
    const accounts = web3.eth.getAccounts((err, acc) => {
      this.accounts = acc;
      const balanceArray = this.balance;
      for (let i = 0; i < acc.length; i++) {
        const weiBalance = web3.eth.getBalance(acc[i]);
        // @ts-ignore
        let convertToEth = web3.fromWei(weiBalance.toNumber(), "ether");
        balanceArray.push(convertToEth + " ETH");
      }
    });
  }

  mounted() {
    this.getAccountBalance();
  }
}
</script>

<style>
.table {
  margin-left: auto;
  margin-right: auto;
}
</style>
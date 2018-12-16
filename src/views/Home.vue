<template>
  <div class="home">
    <table class="table">
      <tr>
        <th>Address</th>
        <th>Balance</th>
        <th>Send</th>
      </tr>
      <tr v-bind:key="index" v-for="(item, index) in accounts">
        <td>{{item}}</td>
        <td>{{balance[index]}}</td>
        <td>
          <section>
            <button class="button" @click="setModal(item)">Send Transaction</button>

            <b-modal :active.sync="isComponentModalActive" has-modal-card>
              <modal-form v-bind="formProps"></modal-form>
            </b-modal>
          </section>
        </td>
      </tr>
    </table>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import Web3 from "web3";
// @ts-ignore
import ModalForm from "@/components/ModalForm";


export default Vue.extend({
  components: {
    ModalForm
  },
  data() {
    return {
      isComponentModalActive: false,
      formProps: {
        amount: null,
        fromAddress: "",
        accounts: []
      },
      accounts: [],
      balance: []
    };
  },
  methods: {
      // @ts-ignore
      setModal(item){
          this.formProps.fromAddress = item;
          this.isComponentModalActive = true
      }
  },
  created() {
    const web3 = new Web3(
      new Web3.providers.HttpProvider("http://127.0.0.1:7545/")
    );
    const accounts = web3.eth.getAccounts((err, acc) => {
        // @ts-ignore
      this.accounts = acc;
      // @ts-ignore
      this.formProps.accounts = acc;
      const balanceArray = this.balance;
      for (let i = 0; i < acc.length; i++) {
        const weiBalance = web3.eth.getBalance(acc[i]);
        // @ts-ignore
        balanceArray.push(web3.fromWei(weiBalance.toNumber(), "ether") + " ETH");
      }
    });
  }
});
</script>

<style>
    .table {
      margin-left: auto;
      margin-right: auto;
    }
</style>

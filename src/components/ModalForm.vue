<template>
  <form action>
    <div class="modal-card" style="width: 1000px; height: 500px">
      <header class="modal-card-head">
        <p class="modal-card-title">Send Transaction</p>
      </header>
      <section class="modal-card-body">
        <b-field>
          <b-dropdown v-model="addressSend" hoverable>
            <button class="button is-info" slot="trigger">
              <span>Select address</span>
              <b-icon icon="menu-down"></b-icon>
            </button>

            <b-dropdown-item v-for="item in accounts" :value="item" v-bind:key="item">{{item}}</b-dropdown-item>
          </b-dropdown>
        </b-field>

        <b-field label="Amount of Ether">
          <b-input
            v-model="amountSend"
            type="number"
            :value="amount"
            placeholder="Ether amount"
            required
          ></b-input>
        </b-field>
      </section>

      <footer class="modal-card-foot">
        <button class="button" type="button" @click="$parent.close()">Close</button>
        <button @click="handler" class="button is-primary">Send</button>
      </footer>
    </div>
  </form>
</template>
<script lang="ts">
import Vue from "vue";
import Web3 from "web3";
export default Vue.extend({
  props: ["amount", "fromAddress", "accounts"],
  data() {
    return {
      addressSend: "",
      amountSend: 1
    };
  },
  methods: {
    handler() {
      this.doThis();
      // @ts-ignore
      this.$parent.close();
    },
    doThis() {
      let web3 = new Web3(
        new Web3.providers.HttpProvider("http://127.0.0.1:7545/")
      );
      web3.eth.sendTransaction({
        from: this.fromAddress,
        to: this.addressSend,
        // @ts-ignore
        value: web3.toWei(this.amountSend, "ether")
      });
    }
  }
});
</script>
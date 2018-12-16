<template>
  <form action>
    <div class="modal-card" style="width: 1000px; height: 500px">
      <header class="modal-card-head">
        <p class="modal-card-title">Send Transaction</p>
      </header>
      <section class="modal-card-body">
        <span class="select_address">To:</span>
        <span>{{addressSend}}</span>
        <br>
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
        <button
          @click="sendTransaction() + $parent.close() + reload()"
          class="button is-primary"
        >Send</button>
      </footer>
    </div>
  </form>
</template>
<script lang="ts">
import Web3 from "web3";
import { Vue, Component, Prop } from "vue-property-decorator";
@Component
export default class ModalForm extends Vue {
  @Prop() amount: number;
  @Prop() fromAddress: string;
  @Prop() accounts: string;
  

  addressSend: string = "";
  amountSend: number = 1;

  sendTransaction() {
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

  reload() {
    setTimeout(function() {
      window.location.reload(true);
    }, 500);
  }
}
</script>

<style lang="scss">
.select_address {
  font-size: 16px;
  font-weight: bold;
}
</style>

<template>
  <div>
      <button class="button" v-on:click="polkaConnect">Connect wallet</button>
      <p >{{ name }}: {{ address }}</p> <!-- This takes in the account name and the account address and displays it to the user after there has been a connection to PolkadotJS -->
      <button class="button" v-on:click="injection">Inject</button>
  </div>
</template>
<style>
  @import './home.css';
</style>

<script>
// import Identicon from "@polkadot/vue-identicon";
// import keyring from "@polkadot/ui-keyring";
// import { cryptoWaitReady } from "@polkadot/util-crypto";

import { ApiPromise } from '@polkadot/api';


export default {
  data: function () {
    let address;
    let name;
    let network;
    return {
      address,
      name,
      network
    }
  },
  methods: {
    async polkaConnect() {
        // Create a new instance of the api
  const api = await ApiPromise.create();
  // get the chain information
  const chainInfo = await api.registry.getChainProperties()

  console.log(chainInfo);
    }
  },
};
</script>
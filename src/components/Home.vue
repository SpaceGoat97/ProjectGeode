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

const { ApiPromise, WsProvider } = require('@polkadot/api');
import { web3Accounts, web3Enable, web3FromAddress } from '@polkadot/extension-dapp';
import { spec } from '@edgeware/node-types';

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
        // The address we use to use for signing, as injected
        const SENDER = 'j4n4fPz3JKu7ahw2zgNjPLbjs1W14Rp7DWudkQGp3uj9unx';

        const allInjected = await web3Enable("Geode"); // We want to access the PolkadotJS signer. We named it Geode and it'll trigger the PolkadotJS signer

        const allAccounts = await web3Accounts(); // We want to get all the accounts
        console.log(allInjected, allAccounts);

        // finds an injector for an address
        const injector = await web3FromAddress(SENDER);

        const api = await ApiPromise.create({ 
            provider : new WsProvider('wss://mainnet1.edgewa.re'),
            typesBundle: spec.typesBundle,
          });

        // sign and send our transaction - notice here that the address of the account
        // (as retrieved injected) is passed through as the param to the `signAndSend`,
        // the API then calls the extension to present to the user and get it signed.
        // Once complete, the api sends the tx + signature via the normal process
        api.tx.balances
          .transfer('5Dcs7PM6VUwLadsAjdUCwveUMr1nvUU2i8uWAfup1yio1R8W', 1)
          .signAndSend(SENDER, { signer: injector.signer }, (status) => { console.log(status) });
    }
    
  },
};
</script>
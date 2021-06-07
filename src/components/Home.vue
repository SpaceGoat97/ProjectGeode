<template>
  <div>
      <button class="button" v-on:click="connectWallet">Connect wallet</button>
      <p >{{ this.name }}: {{ this.address }}</p> <!-- This takes in the account name and the account address and displays it to the user after there has been a connection to PolkadotJS -->
      <button class="button" v-on:click="stakeBronze">Stake 50 EDG</button>
      <button class="button" v-on:click="stakeSilver">Stake 500 EDG</button>
      <button class="button" v-on:click="stakeGold">Stake 5000 EDG</button>
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
    let address, injector, name, api;
    let network = 'wss://mainnet1.edgewa.re';
    let SENDER = 'j4n4fPz3JKu7ahw2zgNjPLbjs1W14Rp7DWudkQGp3uj9unx';
    let unit = '1000000000000000000';
    return {
      address,
      injector,
      api,
      name,
      network,
      SENDER,
      unit
    }
  },
  methods: {
    async connectWallet() {
        // The address we use to use for signing, as injected

        const allInjected = await web3Enable("Geode"); // We want to access the PolkadotJS signer. We named it Geode and it'll trigger the PolkadotJS signer

        const currentAccount = await web3Accounts(); // We want to get all the accounts
        console.log(currentAccount)
        this.address = currentAccount[0].address
        
        console.log(allInjected, this.accounts);
        this.name = currentAccount[0].meta.name;

        // finds an injector for an address
        this.injector = await web3FromAddress(this.address);

        this.api = await ApiPromise.create({ 
            provider : new WsProvider(this.network),
            typesBundle: spec.typesBundle,
          });
    },
    async stakeBronze(){
            // The address we use to use for signing, as injected
        const allInjected = await web3Enable("Geode"); // We want to access the PolkadotJS signer. We named it Geode and it'll trigger the PolkadotJS signer

        const currentAccount = await web3Accounts(); // We want to get all the accounts
        console.log(currentAccount)
        this.address = currentAccount[0].address
        
        console.log(allInjected, this.accounts);
        this.name = currentAccount[0].meta.name;

        // finds an injector for an address
        this.injector = await web3FromAddress(this.address);

        this.api = await ApiPromise.create({ 
            provider : new WsProvider(this.network),
            typesBundle: spec.typesBundle,
          });
        const now = await this.api.query.timestamp.now();
        console.log(now)

        const { nonce, data: balance } = await this.api.query.system.account(this.address);
        console.log(nonce, balance)

        this.api.tx.balances
          .transfer(this.address, '50000000000000000000')
          .signAndSend(this.SENDER, { signer: this.injector.signer }, (status) => { console.log(status) })
          .then((status)=>{
            console.log(status)
            
          })
    },
    async stakeSilver(){
                  // The address we use to use for signing, as injected
        const allInjected = await web3Enable("Geode"); // We want to access the PolkadotJS signer. We named it Geode and it'll trigger the PolkadotJS signer

        const currentAccount = await web3Accounts(); // We want to get all the accounts
        console.log(currentAccount)
        this.address = currentAccount[0].address
        
        console.log(allInjected, this.accounts);
        this.name = currentAccount[0].meta.name;

        // finds an injector for an address
        this.injector = await web3FromAddress(this.address);

        this.api = await ApiPromise.create({ 
            provider : new WsProvider(this.network),
            typesBundle: spec.typesBundle,
          });
        const now = await this.api.query.timestamp.now();
        console.log(now)

        const { nonce, data: balance } = await this.api.query.system.account(this.address);
        console.log(nonce, balance)
        this.api.tx.balances
          .transfer(this.address, '500000000000000000000')
          .signAndSend(this.SENDER, { signer: this.injector.signer }, (status) => { console.log(status) });
    },
    async stakeGold(){
                  // The address we use to use for signing, as injected
        const allInjected = await web3Enable("Geode"); // We want to access the PolkadotJS signer. We named it Geode and it'll trigger the PolkadotJS signer

        const currentAccount = await web3Accounts(); // We want to get all the accounts
        console.log(currentAccount)
        this.address = currentAccount[0].address
        
        console.log(allInjected, this.accounts);
        this.name = currentAccount[0].meta.name;

        // finds an injector for an address
        this.injector = await web3FromAddress(this.address);

        this.api = await ApiPromise.create({ 
            provider : new WsProvider(this.network),
            typesBundle: spec.typesBundle,
          });
        const now = await this.api.query.timestamp.now();
        console.log(now)

        const { nonce, data: balance } = await this.api.query.system.account(this.address);
        console.log(nonce, balance)
        this.api.tx.balances
          .transfer(this.address, '5000000000000000000000')
          .signAndSend(this.SENDER, { signer: this.injector.signer }, (status) => { console.log(status) });
    }
  },
  
};
</script>
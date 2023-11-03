<script setup lang="ts">
//@ts-ignore
import pkg from "https://esm.run/@walletconnect/ethereum-provider@2.9.0/dist/index.umd.js"
import Web3 from 'web3'

let ethereumProvider: pkg.EthereumProvider;
let web3;

const projectId = '0183a45f658feff9f04a07e04c0b8c60';

async function login() {
  if (!ethereumProvider) {
    ethereumProvider = await pkg.EthereumProvider.init({
      projectId,
      showQrModal: true,
      chains: [1],
      metadata: {
        name: "My Dapp",
        description: "My Dapp description",
        url: "https://my-dapp.com",
        icons: ["https://my-dapp.com/logo.png"],
      },
    });

    // 6. Set up connection listener
    ethereumProvider.on("connect", () =>
      console.info(ethereumProvider.accounts)
    );
  }

  await ethereumProvider.enable()

  web3 = new Web3(ethereumProvider)
  const accounts = await web3.eth.getAccounts()
  return accounts[0]
}
</script>

<template>
  <div id="search">
    <button @click="login()">CONNECT</button>
  </div>
</template>

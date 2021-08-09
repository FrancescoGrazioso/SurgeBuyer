<template>
  <div class="container">
    <img width="100%" src="@/assets/surge-logo.png">
    <h2 style="margin-left: auto; margin-right: auto; text-align: center">Mobile version is coming soon!</h2>
  </div>
</template>

<script>
import WalletConnectProvider from "@walletconnect/web3-provider";
import Web3 from "web3"


//  Create WalletConnect Provider
const provider = new WalletConnectProvider({
  rpc: {
    56:  "https://bsc-dataseed1.binance.org/"
  },
  chainId: 56
});
const web3 = new Web3(provider);
let minABI = [
  // balanceOf
  {
    "constant":true,
    "inputs":[{"name":"_owner","type":"address"}],
    "name":"balanceOf",
    "outputs":[{"name":"balance","type":"uint256"}],
    "type":"function"
  },
  // decimals
  {
    "constant":true,
    "inputs":[],
    "name":"decimals",
    "outputs":[{"name":"","type":"uint8"}],
    "type":"function"
  }
];
const surgeContract = "0xe1e1aa58983f6b8ee8e4ecd206cea6578f036c21"

export default {
  name: 'BuySurge',
  props: {
    msg: String
  },
  data: function () {
    return {
      balance: undefined,
      account: undefined,
      accountToShow: undefined,
      surgeBalance: undefined,
      BNBvalue: 0,
      gas: undefined,
      gasPrice: undefined,
      value: undefined,
      hash: undefined
    }
  },
  watch: {
    async value(val) {
      this.BNBvalue = val
      if (val>0) {
        this.gasPrice = await web3.eth.getGasPrice()
        this.gasPrice = web3.utils.fromWei(this.gasPrice, 'gwei')

        const weiValue = web3.utils.toWei(this.BNBvalue.toString(), 'ether')
        this.gas = await web3.eth.estimateGas({
          to: surgeContract,
          from: this.account,
          value: weiValue,
          chainId: 56,
        }).catch(()=>{})
      }
    }
  },
  methods: {
    walletConnect: async function () {

      await provider.enable()
          .catch(() => {});

      //  Get Accounts
      web3.eth.getAccounts(async(error, accounts) => {
        if (error) alert(error)
        this.account = accounts[0]
        const temp = this.account.slice(0, Math.floor(this.account.length/2))
        const temp2 = this.account.slice(Math.floor(this.account.length/1.3), this.account.length)
        this.accountToShow = temp.slice(0, 5) + '....' + temp2
        const balance = await web3.eth.getBalance(this.account)
        this.balance = web3.utils.fromWei(balance, 'ether')

        const t = new web3.eth.Contract(minABI, surgeContract)
        const sfmBal = await t.methods.balanceOf(this.account).call()
        this.surgeBalance = sfmBal
      })

    },
    sendTransaction :  function () {

      if (confirm("You are going to swap " + this.value + " BNB for SurgeToken,\n open your TrustWallet App then press ok to confirm the transaction")) {
        const weiValue = web3.utils.toWei(this.BNBvalue.toString(), 'ether')
        web3.eth.sendTransaction({
          to: surgeContract,
          from: this.account,
          value: weiValue,
          chainId: 56,
        }, (async (error, hash) => {
          if (error) alert(error)
          this.hash = hash
          const balance = await web3.eth.getBalance(this.account)
          this.balance = web3.utils.fromWei(balance, 'ether')

          const t = new web3.eth.Contract(minABI, surgeContract)
          const sfmBal = await t.methods.balanceOf(this.account).call()
          this.surgeBalance = sfmBal
        }));
      }

    },
    disconnect: async function () {
      await provider.disconnect()
      this.account = undefined
      this.balance = undefined
      this.surgeBalance = undefined
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import url("https://fonts.googleapis.com/css?family=Montserrat:700,500");

</style>

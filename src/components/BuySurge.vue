<template>
  <div class="container-center-horizontal">
    <div class="desktop screen">
      <div class="task-bar">
        <div class="logo">
          <div class="overlap-group-1">
            <img class="surge-logo-1" src="@/assets/surge-logo.png">
            <h1 class="title valign-text-middle">Surge Buyer</h1>
          </div>
        </div>
        <div class="login">
          <div v-on:click="walletConnect" v-if="!account" class="overlap-group">
            <div class="wallet-connect valign-text-middle">WalletConnect</div>
          </div>
          <div v-on:click="disconnect" v-if="account" class="overlap-group">
            <div class="log-out valign-text-middle">Log out</div>
          </div>
        </div>
      </div>
      <img class="task-bar-separator" src="@/assets/taskbar-separatorx.png">
      <div v-if="account">
      <img class="task-bar-separator-1" src="@/assets/taskbar-separator-1.png">
        <div class="balance">
          <div class="your-balance valign-text-middle">Your Balance</div>
          <div class="overlap-group2">
            <div class="refresh">
              <div v-on:click="getBalance" class="overlap-group">
                <div class="refresh-balance valign-text-middle">
                  <div v-if="!loading">Refresh Balance</div>
                  <div v-if="loading" class="lds-ellipsis"><div></div><div></div><div></div><div></div></div>
                </div>
              </div>
            </div>
            <div class="balance-frame">
              <div class="bnb-balance">
                <div class="bnb valign-text-middle">BNB</div>
                <div class="xxxx valign-text-middle">{{balance}}</div>
              </div>
              <div class="surge-balance">
                <div class="surge valign-text-middle">SURGE</div>
                <div class="xxxx-1 valign-text-middle">{{surgeBalance}}</div>
              </div>
            </div>
          </div>
        </div>
        <div class="buy-sell">
          <div class="onoffswitch">
            <input v-model="buy" type="checkbox" name="onoffswitch" class="onoffswitch-checkbox" id="myonoffswitch" tabindex="0" checked>
            <label class="onoffswitch-label" for="myonoffswitch">
              <span class="onoffswitch-inner"></span>
              <span class="onoffswitch-switch"></span>
            </label>
          </div>
          <div class="buy-sell-action">
            <div class="insert-value">
              <div style="cursor: pointer" class="name valign-text-middle">Max</div>
              <input v-model="value" type="number" placeholder="Select the value of ..." class="rectangle-1">
            </div>
            <div class="recap">
              <div class="value">
                <div class="bnb-1 valign-text-middle ">{{buy?'BNB':'SURGE'}}</div>
                <div class="yyyyyyyy valign-text-middle ">{{value?value:0}}</div>
              </div>
              <div class="gas">
                <div class="place valign-text-middle ">GAS PRICE</div>
                <div class="yyyyyyyy valign-text-middle">{{gasPrice?gasPrice:0}}</div>
              </div>
            </div>
            <div class="buy-sell-action-1">
              <div v-if="buy" v-on:click="buySurge" class="overlap-group">
                <div class="buy-surge valign-text-middle">Buy Surge</div>
              </div>
              <div v-if="!buy" v-on:click="sellSurge" class="overlap-group">
                <div class="buy-surge valign-text-middle">Sell Surge</div>
              </div>
            </div>
            <div class="contract-selection">
              <div class="openfalse-optiondefault">
                  <select v-model="contract" class="input">
                    <option value="BNB">SURGE BNB</option>
                    <option value="USD">SURGE USD</option>
                  </select>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
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
let minABI = [{"inputs":[],"stateMutability":"nonpayable","type":"constructor"},{"anonymous":false,"inputs":[{"indexed":true,"internalType":"address","name":"owner","type":"address"},{"indexed":true,"internalType":"address","name":"spender","type":"address"},{"indexed":false,"internalType":"uint256","name":"value","type":"uint256"}],"name":"Approval","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"internalType":"address","name":"previousOwner","type":"address"},{"indexed":true,"internalType":"address","name":"newOwner","type":"address"}],"name":"OwnershipTransferred","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"internalType":"address","name":"from","type":"address"},{"indexed":true,"internalType":"address","name":"to","type":"address"},{"indexed":false,"internalType":"uint256","name":"value","type":"uint256"}],"name":"Transfer","type":"event"},{"inputs":[{"internalType":"address","name":"holder","type":"address"},{"internalType":"address","name":"spender","type":"address"}],"name":"allowance","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"address","name":"spender","type":"address"},{"internalType":"uint256","name":"amount","type":"uint256"}],"name":"approve","outputs":[{"internalType":"bool","name":"","type":"bool"}],"stateMutability":"nonpayable","type":"function"},{"inputs":[{"internalType":"address","name":"account","type":"address"}],"name":"balanceOf","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"calculatePrice","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"decimals","outputs":[{"internalType":"uint8","name":"","type":"uint8"}],"stateMutability":"pure","type":"function"},{"inputs":[],"name":"geUnlockTime","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"getBNBQuantityInContract","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"address","name":"holder","type":"address"}],"name":"getValueOfHoldings","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"hyperInflatePrice","outputs":[{"internalType":"bool","name":"","type":"bool"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"uint256","name":"time","type":"uint256"}],"name":"lock","outputs":[],"stateMutability":"nonpayable","type":"function"},{"inputs":[],"name":"name","outputs":[{"internalType":"string","name":"","type":"string"}],"stateMutability":"pure","type":"function"},{"inputs":[],"name":"owner","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"renounceOwnership","outputs":[],"stateMutability":"nonpayable","type":"function"},{"inputs":[{"internalType":"uint256","name":"tokenAmount","type":"uint256"}],"name":"sell","outputs":[{"internalType":"bool","name":"","type":"bool"}],"stateMutability":"nonpayable","type":"function"},{"inputs":[],"name":"sellFee","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"spreadDivisor","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"symbol","outputs":[{"internalType":"string","name":"","type":"string"}],"stateMutability":"pure","type":"function"},{"inputs":[],"name":"totalSupply","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"address","name":"recipient","type":"address"},{"internalType":"uint256","name":"amount","type":"uint256"}],"name":"transfer","outputs":[{"internalType":"bool","name":"","type":"bool"}],"stateMutability":"nonpayable","type":"function"},{"inputs":[],"name":"transferFee","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"address","name":"sender","type":"address"},{"internalType":"address","name":"recipient","type":"address"},{"internalType":"uint256","name":"amount","type":"uint256"}],"name":"transferFrom","outputs":[{"internalType":"bool","name":"","type":"bool"}],"stateMutability":"nonpayable","type":"function"},{"inputs":[{"internalType":"address","name":"newOwner","type":"address"}],"name":"transferOwnership","outputs":[],"stateMutability":"nonpayable","type":"function"},{"inputs":[],"name":"unlock","outputs":[],"stateMutability":"nonpayable","type":"function"},{"stateMutability":"payable","type":"receive"}];
const surgeContract = {
  BNB:"0xe1e1aa58983f6b8ee8e4ecd206cea6578f036c21",
  USD:"0x14fEe7d23233AC941ADd278c123989b86eA7e1fF",
}

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
      hash: undefined,
      buy: true,
      contract: '',
      loading: false
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
          to: surgeContract['BNB'],
          from: this.account,
          value: weiValue,
          chainId: 56,
        }).catch(()=>{})
      } else {
        this.gas = 0
      }
    }
  },
  methods: {
    walletConnect: async function () {

      await provider.enable().then(
          //  Get Accounts
          web3.eth.getAccounts(async(error, accounts) => {
            if (error) alert(error)
            this.account = accounts[0]
            const temp = this.account.slice(0, Math.floor(this.account.length/2))
            const temp2 = this.account.slice(Math.floor(this.account.length/1.3), this.account.length)
            this.accountToShow = temp.slice(0, 5) + '....' + temp2
            const balance = await web3.eth.getBalance(this.account)
            this.balance = web3.utils.fromWei(balance, 'ether')

            const t = new web3.eth.Contract(minABI, surgeContract['BNB'])
            const sfmBal = await t.methods.balanceOf(this.account).call()
            this.surgeBalance = sfmBal
          })
      )
          .catch(() => {
          });
    },
    getBalance: async function () {
      this.loading = true
      const balance = await web3.eth.getBalance(this.account)
      this.balance = web3.utils.fromWei(balance, 'ether')

      const t = new web3.eth.Contract(minABI, surgeContract['BNB'])
      const sfmBal = await t.methods.balanceOf(this.account).call()
      this.surgeBalance = sfmBal
      this.loading = false;
    },
    buySurge :  function () {
      if (this.contract && this.contract.trim()!='') {
        const weiValue = web3.utils.toWei(this.BNBvalue.toString(), 'ether')
        web3.eth.sendTransaction({
          to: surgeContract[this.contract],
          from: this.account,
          value: weiValue,
          chainId: 56,
        }, (async (error, hash) => {
          if (error) alert(error)
          this.hash = hash
          const balance = await web3.eth.getBalance(this.account)
          this.balance = web3.utils.fromWei(balance, 'ether')

          const t = new web3.eth.Contract(minABI, surgeContract[this.contract])
          const sfmBal = await t.methods.balanceOf(this.account).call()
          this.surgeBalance = sfmBal
        }));
      } else {
        alert('Please select wich contract of surge you are buying')
      }
    },
  sellSurge: async function () {
    const Contract = new web3.eth.Contract(minABI, surgeContract[this.contract])

    const fromAddress = this.account

    if (!fromAddress) {
      throw new Error("From address is empty")
    }

    // call sell function
    return Contract.methods.sell(this.value).send({
      from: fromAddress
    });
  },
    disconnect: async function () {
      await provider.disconnect()
      await provider.close()
      this.account = undefined
      this.balance = undefined
      this.surgeBalance = undefined
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import url("https://fonts.googleapis.com/css?family=Montserrat:700|Roboto:300,300italic");

.desktop {
  background-color:
      #1A1A1A;
  position: relative;
  width: 1920px;
}

.desktop .task-bar {
  height: 150px;
  left: 0;
  position: absolute;
  top: 0;
  width: 1920px;
}

.desktop .logo {
  height: 150px;
  left: 0;
  position: absolute;
  top: 0;
  width: 504px;
}

.desktop .overlap-group-1 {
  height: 150px;
  position: relative;
  width: 467px;
  cursor: pointer;
}

.desktop .surge-logo-1 {
  height: 150px;
  left: 0;
  object-fit: cover;
  position: absolute;
  top: 0;
  width: 150px;
}

.desktop .title {
  color: white;
  font-family: Montserrat;
  font-size: x-large;
  font-weight: 700;
  height: 59px;
  left: 145px;
  letter-spacing: 0;
  position: absolute;
  text-align: center;
  top: 45px;
  width: 322px;
}

.desktop .login {
  height: 150px;
  left: 1540px;
  position: absolute;
  top: 0;
  width: 380px;
}

.desktop .overlap-group {
  background-color: #21BBB1;
  border-radius: 68px;
  height: 72px;
  left: 52px;
  position: relative;
  top: 39px;
  width: 276px;
  cursor: pointer;
}

.desktop .wallet-connect {
  height: 29px;
  left: 73px;
  letter-spacing: 0;
  position: absolute;
  text-align: center;
  top: 22px;
  font-family: Montserrat;
}

.desktop .log-out {
  height: 29px;
  left: 107px;
  letter-spacing: 0;
  position: absolute;
  text-align: center;
  top: 22px;
  font-family: Montserrat;
}

.desktop .task-bar-separator {
  height: 1px;
  left: 192px;
  position: absolute;
  top: 150px;
  width: 1536px;

}

.desktop .task-bar-separator-1 {
  height: 700px;
  left: 1178px;
  position: absolute;
  top: 214px;
  width: 1px;
}

.desktop .balance {
  height: 700px;
  left: 1269px;
  position: absolute;
  top: 214px;
  font-family: Montserrat;
  width: 421px;
}

.desktop .your-balance {
  height: 29px;
  left: 128px;
  letter-spacing: 0;
  position: absolute;
  text-align: center;
  top: 21px;
}

.desktop .overlap-group2 {
  height: 525px;
  left: 21px;
  position: absolute;
  top: 153px;
  width: 380px;
}

.desktop .refresh {
  height: 150px;
  left: 0;
  position: absolute;
  top: 375px;
  width: 380px;
}

.desktop .refresh-balance {
  height: 29px;
  left: 75px;
  letter-spacing: 0;
  position: absolute;
  text-align: center;
  top: 22px;
}

.desktop .balance-frame {
  height: 393px;
  left: 14px;
  position: absolute;
  top: 0;
  width: 352px;
}

.desktop .bnb-balance {
  height: 41px;
  left: 14px;
  position: absolute;
  top: 191px;
  width: 338px;
}

.desktop .bnb {
  height: 29px;
  left: 270px;
  letter-spacing: 0;
  position: absolute;
  text-align: center;
  top: 6px;
  font-family: Montserrat;
}

.desktop .xxxx {
  height: 29px;
  left: 7px;
  letter-spacing: 0;
  position: absolute;
  text-align: right;
  top: 6px;
  width: 215px;
  font-family: Montserrat;
}

.desktop .surge-balance {
  height: 41px;
  left: 7px;
  overflow: hidden;
  position: absolute;
  top: 82px;
  width: 338px;
}

.desktop .surge {
  height: 29px;
  left: 251px;
  letter-spacing: 0;
  position: absolute;
  text-align: center;
  top: 6px;
  font-family: Montserrat;
}

.desktop .xxxx-1 {
  height: 29px;
  left: 13px;
  letter-spacing: 0;
  position: absolute;
  text-align: right;
  top: 6px;
  width: 215px;
  font-family: Montserrat;
}

.desktop .buy-sell {
  height: 706px;
  left: 102px;
  position: absolute;
  top: 214px;
  width: 1096px;
  font-family: Montserrat;

}

.desktop .switch {
  background-color:
      #c4c4c4;
  height: 44px;
  left: 482px;

}

.desktop .buy-sell-action {
  height: 615px;
  left: 0;
  position: absolute;
  top: 91px;
  width: 1096px;
}

.desktop .insert-value {
  height: 89px;
  left: 133px;
  position: absolute;
  top: 0;
  width: 829px;
}

.desktop .name {
  height: 29px;
  left: 654px;
  letter-spacing: 0;
  position: absolute;
  text-align: center;
  top: 32px;
}

.desktop .rectangle-1 {
  background-color:
      #edeff2;
  border-radius: 12px;
  height: 48px;
  left: 123px;
  position: absolute;
  top: 21px;
  width: 519px;
}

.desktop .recap {
  height: 238px;
  left: 133px;
  position: absolute;
  top: 165px;
  width: 829px;
}

.desktop .value {
  height: 59px;
  left: 45px;
  position: absolute;
  top: 40px;
  width: 739px;
}

.desktop .bnb-1 {
  height: 29px;
  left: 632px;
  letter-spacing: 0;
  position: absolute;
  text-align: center;
  top: 18px;
  font-family: Montserrat;
}

.desktop .yyyyyyyy {
  height: 29px;
  left: 16px;
  letter-spacing: 0;
  position: absolute;
  top: 15px;
  font-family: Montserrat;
}

.desktop .gas {
  height: 59px;
  left: 45px;
  position: absolute;
  top: 124px;
  width: 739px;
  font-family: Montserrat;
}

.desktop .place {
  height: 29px;
  left: 634px;
  letter-spacing: 0;
  position: absolute;
  text-align: center;
  top: 18px;
}

.desktop .buy-sell-action-1 {
  height: 150px;
  left: 358px;
  position: absolute;
  top: 411px;
  width: 380px;
}

.desktop .buy-surge {
  height: 29px;
  left: 92px;
  letter-spacing: 0;
  position: absolute;
  text-align: center;
  top: 22px;
}

.desktop .contract-selection {
  height: 76px;
  left: 256px;
  position: absolute;
  top: 89px;
  width: 519px;
}

.desktop .openfalse-optiondefault {
  background-color: white;
  border: 1px solid;
  border-radius: 20px;
  height: 36px;
  left: 1px;
  overflow: hidden;
  position: relative;
  top: 20px;
  width: 518px;
}

.desktop .input {
  height: 36px;
  width: 518px;
  border-color: #FFFFFF;
  position: relative;
}

.desktop .default {;
  font-family: Montserrat;
  font-size: small;
  font-style: italic;
  font-weight: 300;
  left: 15px;
  letter-spacing: 0;
  position: absolute;
  top: 10px;
  width: 464px;
}

.desktop .accordion-arrow-light {
  height: 16px;
  left: 487px;
  position: absolute;
  top: 10px;
  width: 16px;
}

.desktop .vector {
  height: 9px;
  left: 1px;
  position: absolute;
  top: 4px;
  width: 14px;
}

.screen a {
  display: contents;
  text-decoration: none;
}

.container-center-horizontal {
  display: flex;
  flex-direction: row;
  justify-content: center;
  pointer-events: none;
  width: 100%;
}

.container-center-horizontal > * {
  flex-shrink: 0;
  pointer-events: auto;
}

.valign-text-middle {
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.onoffswitch {
  position: absolute;
  top: 0;
  width: 122px;
  left: 480px;
}
.onoffswitch-checkbox {
  position: absolute;
  opacity: 0;
  pointer-events: none;
}
.onoffswitch-label {
  display: block; overflow: hidden; cursor: pointer;
  border: 2px solid #999999; border-radius: 50px;
}
.onoffswitch-inner {
  display: block; width: 200%; margin-left: -100%;
  transition: margin 0.3s ease-in 0s;
}
.onoffswitch-inner:before, .onoffswitch-inner:after {
  display: block; float: left; width: 50%; height: 44px; padding: 0; line-height: 44px;
  font-size: 17px; color: white; font-family: Trebuchet, Arial, sans-serif; font-weight: bold;
  box-sizing: border-box;
}
.onoffswitch-inner:before {
  content: "BUY";
  padding-left: 10px;
  background-color: #21BBB1; color: #FFFFFF;
}
.onoffswitch-inner:after {
  content: "SELL";
  padding-right: 10px;
  background-color: #FFFFFF; color: #21BBB1;
  text-align: right;
}
.onoffswitch-switch {
  display: block; width: 27px; margin: 8.5px;
  background: #FFFFFF;
  position: absolute; top: 0; bottom: 0;
  right: 74px;
  border: 2px solid #999999; border-radius: 50px;
  transition: all 0.3s ease-in 0s;
}
.onoffswitch-checkbox:checked + .onoffswitch-label .onoffswitch-inner {
  margin-left: 0;
}
.onoffswitch-checkbox:checked + .onoffswitch-label .onoffswitch-switch {
  right: 0px;
}

.lds-ellipsis {
  display: inline-block;
  position: relative;
  width: 80px;
  height: 80px;
}
.lds-ellipsis div {
  position: absolute;
  top: 8px;
  width: 13px;
  height: 13px;
  border-radius: 50%;
  background: #fff;
  animation-timing-function: cubic-bezier(0, 1, 1, 0);
}
.lds-ellipsis div:nth-child(1) {
  left: 34px;
  animation: lds-ellipsis1 0.6s infinite;
}
.lds-ellipsis div:nth-child(2) {
  left: 34px;
  animation: lds-ellipsis2 0.6s infinite;
}
.lds-ellipsis div:nth-child(3) {
  left: 58px;
  animation: lds-ellipsis2 0.6s infinite;
}
.lds-ellipsis div:nth-child(4) {
  left: 82px;
  animation: lds-ellipsis3 0.6s infinite;
}
@keyframes lds-ellipsis1 {
  0% {
    transform: scale(0);
  }
  100% {
    transform: scale(1);
  }
}
@keyframes lds-ellipsis3 {
  0% {
    transform: scale(1);
  }
  100% {
    transform: scale(0);
  }
}
@keyframes lds-ellipsis2 {
  0% {
    transform: translate(0, 0);
  }
  100% {
    transform: translate(24px, 0);
  }
}

</style>

<template>
  <div class="home">
    <h1>Dan Fulcher's Sequel Group Test</h1>
    <form name="change-calc" @submit.prevent="calcChange(pennies)">
      <input v-model="input" placeholder="Amount"/>
      <button type="submit">Calculate</button>
      <ul>
        <li :key="index" v-for="(money, index) in this.output">{{ money }}</li>
      </ul>
    </form>
  </div>
</template>

<script>
function initCoins () {
    return [
        200,
        100,
        50,
        20,
        10,
        5,
        2,
        1
    ]
}
export default {
  name: 'home',
  data () {
      return {
          input: '',
          pennies:'',
          coins: initCoins(),
          output: []
      }
  },
  watch: {
    // watches for when the input field is being typed in.
    // when it is, it clears the output array and resets the coins array.
    // it also sends the input value to the handleData function so it can be formatted correctly.
    input: function () {
      this.output = [],
      this.coins = initCoins(),
      this.handleData(this.input);
    }
  },
  methods: {
      handleData (amount) {
          // If value given starts with a £. It removes £, converts to a float and multiplies by 100.
          // Else it does the same thing, but it won't multiply by 100 unless it has a decimal point.
          if(amount.startsWith('£')) {
              let newVal = parseFloat(amount.replace(/[^0-9-.]/g, ''));
              this.pennies = newVal*100;
          } else {
              let newVal = parseFloat(amount.replace(/[^0-9-.]/g, ''));
              this.pennies = newVal;
              if(newVal % 1 !== 0) {
                  this.pennies = (newVal*100);
              } else {
                  this.pennies = newVal;
              }
          }
      },

      calcChange (amount) {


          if (amount === 0)
          {
              return [];
          }
          else
          {
              // If given amount is larger than largest coin, removes largest coin value from amount and adds to the outputted array.
              // Then runs the function again with leftover amount.
              if (amount >= this.coins[0])
              {
                  let left = (amount - this.coins[0]);
                  this.output.push(new Intl.NumberFormat('en-EN', {
                      style: 'currency',
                      currency: 'GBP',
                      minimumFractionDigits:2,
                      maximumFractionDigits:2
                  }).format(this.coins[0] / 100));
                  return [this.coins[0]].concat( this.calcChange(left));
              }
              // If given amount is NOT larger the largest coin, it removes the largest coin from the array
              // Then runs array again.
              else
              {
                  this.coins.shift();
                  return this.calcChange(amount);
              }
          }

      }
  },

}
</script>

<style lang="scss">
  form {
    input {
      -webkit-appearance:none;
      border:none;
      border-bottom:1px solid #d4d4d4;
      outline:none;
      padding:10px 0;
      margin-right:15px;
      font-size:18px;

    }
    button {
      background:#0090ff;
      padding:10px;
      border:1px solid #0090ff;
      border-radius: 3px;
      color:#fff;
      font-size:18px;
      cursor:pointer;
      &:active, &:focus {
        outline:none;
      }
      &:hover {
        background:#fff;
        color:#0090ff;
      }
    }
    ul {
      padding-left:0;
      li {
        list-style:none;
        font-size:21px;
      }
    }
  }
</style>

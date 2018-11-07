<template lang="html">
  <div class="container my-5">
    <h1 class="text-center">Currency Converter</h1>
    <div class="row">
      <div class="col-md-6">
        <label for="">From</label>
        <select class="form-control" name="" v-model="from">
          <option :value="currency.id" v-for="currency in formattedCurrencies">{{currency.currencyName}}</option>
        </select>
      </div>
      <div class="col-md-6">
        <label for="">To</label>
        <select class="form-control" name="" v-model="to">
          <option :value="currency.id" v-for="currency in formattedCurrencies">{{currency.currencyName}}</option>
        </select>
      </div>
    </div>
    <div class="row">
      <div class="com-md-6 offset-md-3">
        <input type="text" name="" value="" class="form-control my-5" placeholder="Amount" v-model="amount">
        <div class="text-center">
          <button type="button" name="button" class="btn btn-block btn-primary" @click="convertCurrency()" :disabled="disable">{{ loading ? 'Converting..' : 'Convert'}}</button>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col-md-6 offset-md-3">
        <h1 class="text-center display-2 mt-5">{{calculateResult}}</h1>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  mounted(){
    this.getCurrencies()
  },
  data: function(){
    return{
      currencies: {},
      amount:0,
      from :'EUR',
      to:'USD',
      result: 0,
      loading: false
    }
  },
  methods:{
    getCurrencies(){
      const currencies = localStorage.getItem('currencies')
      if(currencies){
        this.currencies = JSON.parse(currencies);
        return;
      }
      axios.get('https://free.currencyconverterapi.com/api/v6/currencies')
        .then(response => {
          this.currencies = response.data.results;
          localStorage.setItem('currencies', JSON.stringify(response.data.results))
        })
    },
    convertCurrency(){
      const key = `${this.from}_${this.to}`;
      this.loading = true;
      axios.get(`https://free.currencyconverterapi.com/api/v6/convert?q=${key}`)
        .then(response => {
          this.loading = false;
          this.result = response.data.results[key].val
        })
    }
  },
  computed: {
    formattedCurrencies(){
      return Object.values(this.currencies)
    },
    calculateResult(){
      return (Number(this.amount) * this.result).toFixed(3);
    },
    disable(){
      return this.amount === 0 || this.amount === '' || this.loading;
    }
  },
  watch:{
    from(){
      this.result = 0
    },
    to(){
      this.result = 0
    }
  }
}
</script>

<style lang="css">
</style>

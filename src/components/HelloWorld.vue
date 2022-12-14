<template>
  <v-row>
    <v-text-field
        pattern="\d*"
        label="대출금액"
        v-model="money"
        @input="changeNum($event.target.value)"
        suffix="원"
    ></v-text-field>
  </v-row>
  <v-row class="mb-6">
    <div class="d-flex" style="gap: 0.5rem">
      <v-btn variant="tonal" @click="incrementMoney(100000)">십만</v-btn>
      <v-btn variant="tonal" @click="incrementMoney(1000000)">백만</v-btn>
      <v-btn variant="tonal" @click="incrementMoney(10000000)">천만</v-btn>
      <v-btn variant="tonal" @click="incrementMoney(100000000)">일억</v-btn>
      <v-btn color="error" variant="tonal" @click="this.money = 0">정정</v-btn>
    </div>
  </v-row>
  <v-row>
    <v-text-field
        pattern="\d*"
        label="대출기간"
        v-model="period"
        suffix="개월"
    ></v-text-field>
  </v-row>
  <v-row class="mb-6">
    <div class="d-flex" style="gap: 0.5rem">
      <v-btn variant="tonal" @click="incrementPeriod(1)">1개월</v-btn>
      <v-btn variant="tonal" @click="incrementPeriod(6)">6개월</v-btn>
      <v-btn variant="tonal" @click="incrementPeriod(12)">1년</v-btn>
      <v-btn variant="tonal" @click="incrementPeriod(24)">2년</v-btn>
      <v-btn color="error" variant="tonal" @click="this.period = 0">정정</v-btn>
    </div>
  </v-row>
  <v-row>
    <v-text-field
        label="대출금리"
        v-model="interest"
        suffix="%"
    ></v-text-field>
  </v-row>
  <v-row class="mb-6">
    <div class="d-flex" style="gap: 0.5rem">
      <v-btn variant="tonal" @click="incrementInterest(0.1)">0.1%</v-btn>
      <v-btn variant="tonal" @click="incrementInterest(0.5)">0.5%</v-btn>
      <v-btn variant="tonal" @click="incrementInterest(1)">1%</v-btn>
      <v-btn variant="tonal" @click="incrementInterest(5)">5%</v-btn>
      <v-btn color="error" variant="tonal" @click="this.interest = 0">정정</v-btn>
    </div>
  </v-row>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      money: 0,
      period: 0,
      interest: 0
    }
  },
  methods: {
    incrementMoney(num) {
      this.money = this.comma(this.unComma(this.money) + num);
    },
    incrementPeriod(num) {
      this.period += num;
    },
    incrementInterest(num) {
      this.interest += num;
    },
    changeNum: function (value) {
      this.money = this.comma(this.unComma(value));
    },
    comma(str) {
      str = String(str);
      return str.replace(/(\d)(?=(?:\d{3})+(?!\d))/g, '$1,');
    },
    unComma(str) {
      str = String(str);
      return parseInt(str.replace(/[^\d]+/g, ''));
    }
  },
  filters: {
    inputNumberFormat(val) {
      return String(val).replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    }
  }
}
</script>

<style scoped>

</style>

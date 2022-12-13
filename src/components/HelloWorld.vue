<template>
  <div>
    <v-text-field label="대출금액" v-model="money"></v-text-field>
    <div class="d-flex" style="gap: 0.5rem">
      <v-btn variant="tonal" @click="incrementMoney(100000)">십만원</v-btn>
      <v-btn variant="tonal" @click="incrementMoney(1000000)">백만원</v-btn>
      <v-btn variant="tonal" @click="incrementMoney(10000000)">천만원</v-btn>
      <v-btn color="error" variant="tonal" @click="this.money = 0">정정</v-btn>
    </div>
  </div>
  <div>
    <v-text-field label="대출기간" v-model="period"></v-text-field>
    <div class="d-flex" style="gap: 0.5rem">
      <v-btn variant="tonal" @click="incrementPeriod(1)">1개월</v-btn>
      <v-btn variant="tonal" @click="incrementPeriod(6)">6개월</v-btn>
      <v-btn variant="tonal" @click="incrementPeriod(12)">1년</v-btn>
      <v-btn variant="tonal" @click="incrementPeriod(24)">2년</v-btn>
      <v-btn color="error" variant="tonal" @click="this.period = 0">정정</v-btn>
    </div>
  </div>
  <div>
    <v-text-field label="대출금리" v-model="interest"></v-text-field>
    <div class="d-flex" style="gap: 0.5rem">
      <v-btn variant="tonal" @click="incrementInterest(0.1)">0.1%</v-btn>
      <v-btn variant="tonal" @click="incrementInterest(0.5)">0.5%</v-btn>
      <v-btn variant="tonal" @click="incrementInterest(1)">1%</v-btn>
      <v-btn variant="tonal" @click="incrementInterest(5)">5%</v-btn>
      <v-btn color="error" variant="tonal" @click="this.interest = 0">정정</v-btn>
    </div>
  </div>
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
      this.money += num;
    },
    incrementPeriod(num) {
      this.period += num;
    },
    incrementInterest(num) {
      this.interest += num;
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>

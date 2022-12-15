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
  <v-row justify="center" class="mb-8">
    <v-btn-toggle
        v-model="exclusive"
        color="deep-purple-accent-3"
        group
    >
      <v-btn variant="outlined">원리금균등상환</v-btn>
      <v-btn variant="outlined">원금균등상환</v-btn>
      <v-btn variant="outlined">원금만기일시상환</v-btn>
    </v-btn-toggle>
  </v-row>
  <v-row class="mb-4">
    <v-btn
        variant="tonal"
        block
        @click="calculation()"
    >계산하기
    </v-btn>
  </v-row>
  <v-row class="mb-4">
    <v-table
        class="w-100 main-table"
    >
      <tbody>
        <tr>
          <td>대출원금</td>
          <td class="text-right">{{ calMoney }} 원</td>
        </tr>
        <tr>
          <td>대출이자</td>
          <td class="text-right text-red">{{ calInterest }} 원</td>
        </tr>
        <tr>
          <td>상환금액</td>
          <td class="text-right">{{ calSum }} 원</td>
        </tr>
      </tbody>
    </v-table>
  </v-row>
  <v-row class="mb-4">
    <v-table
        class="w-100"
        density="compact"
        fixed-header
    >
      <thead>
        <tr>
          <th class="text-center"></th>
          <th class="text-center">월상환금</th>
          <th class="text-center">납입원금</th>
          <th class="text-center">대출이자</th>
          <th class="text-center">잔금</th>
        </tr>
      </thead>
      <tbody>
        <tr
            v-for="(item, index) in items"
            :key="item.name"
        >
          <td class="text-center">{{ index + 1 }}</td>
          <td class="text-center">{{ item.repayment }}</td>
          <td class="text-center">{{ item.original }}</td>
          <td class="text-center">{{ item.interest }}</td>
          <td class="text-center">{{ item.total }}</td>
        </tr>
      </tbody>
    </v-table>
  </v-row>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      money: 0,
      period: 0,
      interest: 0,
      exclusive: 0,
      calMoney: null,
      calInterest: null,
      items: [],
    }
  },
  computed: {
    calSum() {
      const result = this.comma(this.unComma(this.calMoney) + this.unComma(this.calInterest));
      return result === 'NaN' ? 0 : result;
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
    },
    calculation() {
      if (this.exclusive === 2) {
        this.maturity();
      }
    },
    maturity() {
      const obj = this;
      obj.calMoney = obj.money;
      obj.calInterest = obj.comma(obj.unComma(obj.money) * obj.period * (obj.interest / 12 / 100));

      const items = [];

      let totalValue = obj.unComma(obj.calMoney) + obj.unComma(obj.calInterest);
      const monthInterest = (obj.unComma(obj.calInterest) / obj.period).toFixed(0);

      for (let i = 0; i < obj.period - 1; i++) {

        totalValue = totalValue - monthInterest;
        items.push({
          repayment: obj.comma(monthInterest),
          original: 0,
          interest: obj.comma(monthInterest),
          total: obj.comma(totalValue)
        });
      }

      items.push({
        repayment: obj.comma(totalValue),
        original: obj.comma(obj.calMoney),
        interest: obj.comma(totalValue - obj.unComma(obj.calMoney)),
        total: 0
      });

      obj.items = items;
    }
  },
  filters: {
    inputNumberFormat(val) {
      return String(val).replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    },
  }
}
</script>

<style scoped>
.main-table {
  font-weight: bold;
}

.v-btn-group button {
  font-size: 12px;
}
</style>

<template>
  <v-container>
    <v-row justify="center" align="center">
      <v-col cols="12" sm="8" md="10">
        <v-simple-table dark>
          <template v-slot:default>
            <thead>
              <tr>
                <th v-for="n of N + 1" :key="n + uuid()" class="text-left">
                  {{ n - 1 }}
                </th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="items in cTable" :key="items.index + uuid()">
                <td v-for="item in items" :key="item + uuid()">{{ item }}</td>
              </tr>
            </tbody>
          </template>
        </v-simple-table>
      </v-col>
    </v-row>

    <v-row justify="center" align="center">
      <v-col cols="12" sm="8" md="10">
        <v-simple-table dark>
          <template v-slot:default>
            <thead>
              <tr>
                <th v-for="n of M + 1" :key="n" class="text-left">
                  {{ n - 1 }}
                </th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="m of M + 1" :key="m">
                <td v-for="(item,index) in dp[m-1]" :key="item.keys[index]">{{ item[index] }}</td>
              </tr>
            </tbody>
          </template>
        </v-simple-table>
      </v-col>
    </v-row>
    <v-divider />
  </v-container>
</template>

<script lang="ts">
import Vue from "vue";
import {uuid} from "vue-uuid"

export default Vue.extend({
  components: {},
  mounted() {
    this.makeCTable()
    this.culc()
  },
  data() {
    return {
      N: 5 as number,
      M: 3 as number,
      numbers: [9, 1, 2, 3, 9] as number[],
      text: "" as string,
      cTable: [[]] as number[][],
      // dp: [[{val:0,key:""}]] as any,
      dp:   [    {
          0: 0,
          1: 0,
          2: 0,
          3: 0,
          colors: {
            0: '',
            1: '',
            2: '',
            3: '',
          },
          keys: {
            0: '1',
            1: '2',
            2: '3',
            3: '4',
          },
          
          
          }] as any,
    };
  },
  methods: {
    makeCTable() {
      this.cTable = new Array(this.N + 1);
      for (let i = 0; i < this.N + 1; ++i) {
        this.cTable[i] = new Array(this.N + 1);
      }
      for (let i = 0; i <= this.N; ++i) {
        for (let j = i + 1; j <= this.N; ++j) {
          let sum = 0;
          for (let k = i; k < j; ++k) {
            sum += this.numbers[k];
          }
          this.cTable[i][j] = sum / (j - i);
        }
      }
    },
    async culc() {
      this.dp.shift()
      // this.dp = new Array(this.N + 1);
      for (let i = 0; i < this.N + 1; ++i) {
        // this.dp[i] = new Array(this.M + 1).fill(-Number.MAX_VALUE);
         this.dp.push({
          0: -Number.MAX_VALUE,
          1: -Number.MAX_VALUE,
          2: -Number.MAX_VALUE,
          3: -Number.MAX_VALUE,
          colors: {
            0: false,
            1: false,
            2: false,
            3: false,
          },
          keys: {
            0: this.uuid(),
            1: this.uuid(),
            2: this.uuid(),
            3: this.uuid(),
          },
          
          
          })
        // for (let m = 0; m < this.M + 1; ++m) {
        //   this.dp[i][m] = {val: -Number.MAX_VALUE, key: uuid.v4()}
        // }
      }
      this.dp[0][0] = 0;
      for (let i = 0; i <= this.N; ++i) {
        for (let k = 1; k <= this.M; ++k) {
          if (k > i) {
            break;
          }
                      await new Promise((resolve) => setTimeout(resolve, 1000));

          for (let j = 0; j < i; ++j) {
             if (this.dp[i][k] < this.dp[j][k-1] + this.cTable[j][i]) {
               this.dp[i][k] = this.dp[j][k-1]+ this.cTable[j][i]
                     // this.$set(this.dp[0], 0, {val:9, key:"aa"});
                     // this.dp[0][0] =  Object.assign({}, this.dp[0], { val: 1, key: 1111 })

             }
          }
        }
      }

      // todo check all last array values
      console.log(this.dp[this.N][this.M])
    },
    uuid() {
      return uuid.v4()
    }
  },
});
</script>

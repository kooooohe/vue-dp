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
              <tr v-for="items in dp" :key="items.index">
                <td v-for="item in items" :key="item">{{ item }}</td>
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
      dp: [[]] as number[][],
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
    culc() {
      this.dp = new Array(this.N + 1);
      for (let i = 0; i < this.N + 1; ++i) {
        this.dp[i] = new Array(this.M + 1).fill(-Number.MAX_VALUE);
      }
      this.dp[0][0] = 0;
      for (let i = 0; i <= this.N; ++i) {
        for (let k = 1; k <= this.M; ++k) {
          if (k > i) {
            break;
          }
          for (let j = 0; j < i; ++j) {
             if (this.dp[i][k] < this.dp[j][k-1] + this.cTable[j][i]) {
               this.dp[i][k] = this.dp[j][k-1] + this.cTable[j][i]
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

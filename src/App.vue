<script setup>
import json from "./json/data.json";

import { ref } from "vue";
const maxElem = ref(
  json.lastSevenDays.reduce((max, cur) => {
    return max.amount > cur.amount ? max : cur;
  })
);

const graphFigHeight = 150;
const diff = ((1 - json.totalPrevMonth / json.totalThisMonth) * 100).toFixed(1);
</script>

<template>
  <main>
    <h1 class="sr-only">Expenses chart</h1>

    <div class="balance-info">
      <div class="balance-info__label">My balance</div>
      <div class="balance-info__amount">${{ json.balance }}</div>
    </div>
    <div class="spending-info">
      <h2>Spending - Last 7 days</h2>
      <div class="spending-info__chart-container">
        <div v-for="elem in json.lastSevenDays">
          <div
            class="spending-info__chart-container__fig"
            :style="{
              height:
                elem.amount === maxElem.amount
                  ? `${graphFigHeight}px`
                  : `${graphFigHeight * (elem.amount / maxElem.amount)}px`,
              backgroundColor:
                elem.amount === maxElem.amount
                  ? `var(--cyan-hsl)`
                  : `var(--soft-red)`,
            }"
          ></div>
          <div class="spending-info__chart-container__tooltip">
            ${{ elem.amount }}
          </div>
          <div class="spending-info__chart-container__label">
            {{ elem.day }}
          </div>
        </div>
      </div>
      <div class="spending-info__stats-container">
        <div>
          <div class="spending-info__stats-container__total-label">
            Total this month
          </div>
          <div class="spending-info__stats-container__total-amount">
            ${{ json.totalThisMonth }}
          </div>
        </div>
        <div>
          <div class="spending-info__stats-container__diff-amount">
            {{ diff > 0 ? `+${Math.abs(diff)}` : `-${Math.abs(diff)}` }}%
          </div>
          <div class="spending-info__stats-container__diff-label">
            from last month
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped></style>

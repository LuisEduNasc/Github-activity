<template>
  <div class="year-button-list">
    <ul>
      <li v-for="item in getYearList()" :key="item">
        <a-button @click="handleChangeSelectedYear(item)" :type="`${selectedYear === item ? 'primary' : 'text'}`">{{item}}</a-button>
      </li>
    </ul>
  </div>
</template>

<script>
  import { format } from 'date-fns'

  export default {
    name: 'YearList',
    props: {
      selectedYear: Number,
      handleChangeSelectedYear: Function,
    },
    methods: {
      getYearList() {
        let listYear = [];
        for (let i = 0; i < 12; i++) {
          listYear.push(format(new Date(), 'yyyy') - i);
        }
        return listYear;
      }
    },
  }
</script>

<style>
  .year-button-list ul {
    list-style: none;
  }

  .year-button-list button:not(.ant-btn-primary) {
    color: var(--font-primary);
  }
</style>

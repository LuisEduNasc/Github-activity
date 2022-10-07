<template>
  <a-layout>
    <a-layout-header>
      <GraphBoard
        :days="getDaysOfTheYear()"
        :contributions="getContributionsByYear()"
        @handleSelectedDate="handleSelectedDate"
        :selectedYear="selectedYear"
      />
    </a-layout-header>
    <a-layout>
      <a-layout-content>
        <ContributionActivity
          :contributions="contributionsByDate"
          :selectedDate="selectedDate"
          :selectedYear="selectedYear"
          :groupByKey="groupByKey"
        />
      </a-layout-content>
      <a-layout-sider>
        <YearList
          :selectedYear="selectedYear"
          :handleChangeSelectedYear="handleChangeSelectedYear"
        />
      </a-layout-sider>
    </a-layout>
  </a-layout>
</template>

<script>
import {eachDayOfInterval, format, getYear, parseISO} from 'date-fns'

import GraphBoard from './components/GraphBoard.vue'
import ContributionActivity from './components/ContributionActivity.vue'
import YearList from './components/YearList.vue'
import jsonData from '../data/activity.json'

export default {
  name: 'App',
  components: {
    GraphBoard,
    ContributionActivity,
    YearList,
},
  data() {
    return {
      jsonData,
      selectedYear: 2012,
      selectedDate: null,
      contributionsByDate: []
    }
  },
  mounted() {
    this.filterContributions(this.selectedYear);
  },
  methods: {
    getDaysOfTheYear() {
      const result = eachDayOfInterval({
        start: new Date(this.selectedYear, 0, 1),
        end: new Date(this.selectedYear, 11, 31)
      });

      return result;
    },
    getContributionsByYear() {
      const result = this.jsonData.filter(
        (contribution) => getYear(parseISO(contribution.created_at)) === this.selectedYear
      );
      return result;
    },
    handleChangeSelectedYear(year) {
      this.selectedYear = year;
      this.selectedDate = null;
      this.filterContributions(year);
    },
    handleSelectedDate(date) {
      this.selectedDate = date;
      this.filterContributions();
    },
    filterContributions(year) {
      let filteredContributions;

      if (!year && this.selectedDate) {
        filteredContributions = this.jsonData.filter(
          (contribution) => format(new Date(contribution.created_at), 'yyyy-MM-dd') === format(this.selectedDate, 'yyyy-MM-dd')
        );
      } else {
        filteredContributions = this.jsonData.filter(
          (contribution) => format(new Date(contribution.created_at), 'yyyy') == (year || this.selectedYear)
        );
      }

      return this.contributionsByDate = this.groupByKey(filteredContributions, 'type');
    },
    groupByKey(array, key) {
      return array.reduce((acc, curr) => {
        (acc[curr[key]] = acc[curr[key]] || []).push(curr);
        return acc;
      }, {})
    },
  }
}
</script>

<style>
  p {
    margin: 0;
    padding: 0;
    font-size: 16px;
    line-height: 26px;
  }

  .ant-layout {
    --background-primary: #22272e;
    --font-primary: #adbac7;
    --font-secondary: #6e7a87;
    --inactivity-color: #2d333b;
    --one-activity-color: #0e4329;
    --two-activity-color: #026d32;
    --three-activity-color: #26a641;
    --lot-activity-color: #3ad353;

    color: var(--font-primary);
    background: #22272e;
    min-height: 100vw;
    padding: 22px 15vw;
  }

  .ant-layout .ant-layout-header {
    height: auto;
    color: var(--font-primary);
    background: #22272e;
  }

  .ant-layout.ant-layout-has-sider {
    padding: 26px 50px;
  }

  .ant-layout .ant-layout-sider.ant-layout-sider-dark {
    background: #22272e;
    max-width: 120px !important;
    min-width: 120px !important;
    width: 120px !important;
  }
  </style>

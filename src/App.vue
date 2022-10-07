<template>
  <a-layout>
    <a-layout-header>
      <GraphBoard :days="getDaysOfTheYear()" :contributions="getContributionsByYear()" @handleSelectedDate="handleSelectedDate" />
    </a-layout-header>
    <a-layout>
      <a-layout-content>
        <ContributionActivity :contributions="contributionsByDate" :selectedDate="selectedDate" :selectedYear="selectedYear"/>
      </a-layout-content>
      <a-layout-sider>
        <YearList :selectedYear="selectedYear" :handleChangeSelectedYear="handleChangeSelectedYear" />
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
  watch: {
    selectedDate() {
      this.filterContributions();
    }
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
      console.log("🚀 ~ file: App.vue ~ line 59 ~ getContributionsByYear ~ result", result)
      return result;
    },
    handleChangeSelectedYear(year) {
      return this.selectedYear = year;
    },
    handleSelectedDate(date) {
      console.log("🚀 ~ file: App.vue ~ line 64 ~ handleSelectedDate ~ date", date)
      return this.selectedDate = date;
    },
    filterContributions() {
      const filteredContributions = this.jsonData.filter(
        (contribution) => format(new Date(contribution.created_at), 'yyyy-MM-dd') === format(this.selectedDate, 'yyyy-MM-dd')
      );
      console.log("🚀 ~ file: App.vue ~ line 71 ~ filterContributions ~ filteredContributions", filteredContributions)
      return this.contributionsByDate = filteredContributions;
    }
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

<template>
  <div class="header-section">
    <div>
      <p>{{contributions.length}} contributions in {{selectedYear}}</p>
    </div>
    <div class="graph-container">
      <div class="graph">
        <div class="side-header">
          <ul>
            <li style="height: 16px;">
            </li>
            <li style="height: 16px;">
            </li>
            <li>
              <p>Mon</p>
            </li>
            <li style="height: 16px;">
            </li>
            <li>
              <p>Wed</p>
            </li>
            <li style="height: 16px;">
            </li>
            <li>
              <p>Fri</p>
            </li>
            <li style="height: 16px;">
            </li>
          </ul>
        </div>
        <div class="top-header">
          <ul>
            <li>
              <p>Jan</p>
              <ul class="date-list sun">
                <li v-for="item in filterWeekday('Sun')" :key="item" :class="`${formatDateToClass(item)}`">
                  <button
                    :title="`${getContributionsPerDay(item).length} contributions on ${formatDate(item)}`"
                    :class="`contr-count-${getContributionsPerDay(item).length}`"
                    @click="selectDate(item)"
                  >
                  </button>
                </li>
              </ul>
              <ul class="date-list mon">
                <li v-for="item in filterWeekday('Mon')" :key="item" :class="`${formatDateToClass(item)}`">
                  <button
                    :title="`${getContributionsPerDay(item).length} contributions on ${formatDate(item)}`"
                    :class="`contr-count-${getContributionsPerDay(item).length}`"
                    @click="selectDate(item)"
                  >
                  </button>
                </li>
              </ul>
              <ul class="date-list tue">
                <li v-for="item in filterWeekday('Tue')" :key="item" :class="`${formatDateToClass(item)}`">
                  <button
                    :title="`${getContributionsPerDay(item).length} contributions on ${formatDate(item)}`"
                    :class="`contr-count-${getContributionsPerDay(item).length}`"
                    @click="selectDate(item)"
                  >
                  </button>
                </li>
              </ul> <ul class="date-list wed">
                <li v-for="item in filterWeekday('Wed')" :key="item" :class="`${formatDateToClass(item)}`">
                  <button
                    :title="`${getContributionsPerDay(item).length} contributions on ${formatDate(item)}`"
                    :class="`contr-count-${getContributionsPerDay(item).length}`"
                    @click="selectDate(item)"
                  >
                  </button>
                </li>
              </ul> <ul class="date-list thu">
                <li v-for="item in filterWeekday('Thu')" :key="item" :class="`${formatDateToClass(item)}`">
                  <button
                    :title="`${getContributionsPerDay(item).length} contributions on ${formatDate(item)}`"
                    :class="`contr-count-${getContributionsPerDay(item).length}`"
                    @click="selectDate(item)"
                  >
                  </button>
                </li>
              </ul> <ul class="date-list fri">
                <li v-for="item in filterWeekday('Fri')" :key="item" :class="`${formatDateToClass(item)}`">
                  <button
                    :title="`${getContributionsPerDay(item).length} contributions on ${formatDate(item)}`"
                    :class="`contr-count-${getContributionsPerDay(item).length}`"
                    @click="selectDate(item)"
                  >
                  </button>
                </li>
              </ul> <ul class="date-list sat">
                <li v-for="item in filterWeekday('Sat')" :key="item" :class="`${formatDateToClass(item)}`">
                  <button
                    :title="`${getContributionsPerDay(item).length} contributions on ${formatDate(item)}`"
                    :class="`contr-count-${getContributionsPerDay(item).length}`"
                    @click="selectDate(item)"
                  >
                  </button>
                </li>
              </ul>
            </li>
          </ul>
        </div>
      </div>

      <div class="bottom-graph">
        <p>Learn how we count contributions</p>
        <div class="legend-graph">
          <p>Less</p>
          <div>
            <ul>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
            </ul>
          </div>
          <p>More</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { format } from 'date-fns';

export default {
  name: 'GraphBoard',
  props: {
    days: Array,
    contributions: Array,
    selectedYear: Number,
  },
  methods: {
    filterWeekday(weekday) {
      const newArray = this.days.filter(
        (day) => day.toString().split(' ')[0] === weekday
      );
      return newArray;
    },
    formatDate(date) {
      return format(date, 'MMM dd, yyyy');
    },
    formatDateToClass(date) {
      return format(date, 'MMM_dd');
    },
    getContributionsPerDay(date) {
      const dayContribution = this.contributions.filter(
        (contribution) => format(new Date(contribution.created_at), 'MMM dd, yyyy') === format(date, 'MMM dd, yyyy')
      );
      return dayContribution;
    },
    selectDate(date) {
      return this.$emit('handleSelectedDate', date);
    }
  }
}
</script>

<style>
  .header-section {
    position: relative;
  }

  .header-section > div > p {
    margin-bottom: 12px;
  }

  .header-section p {
    font-size: 12px;
  }

  .header-section .graph-container {
    position: relative;
    border: 1px solid #444c56;
    border-radius: 10px 10px 0 0;
    padding: 6px 22px;
    width: fit-content;
    margin: 0 auto;
  }

  .header-section ul {
    list-style: none;
    margin-block-start: 0;
    padding-inline-start: 0;
  }

  .header-section .graph-container .graph {
    display: flex;
    margin-bottom: 16px;
  }

  .header-section .graph-container .graph ul.date-list,
  .header-section .graph-container .graph .top-header ul {
    display: flex;
  }

  .header-section .graph-container .graph ul.date-list li {
    position: relative;
  }

  .header-section .graph-container .graph ul.date-list.sun > li:first-child:not(.Jan_01),
  .header-section .graph-container .graph ul.date-list.mon > li:first-child:not(.Jan_01):not(.Jan_02),
  .header-section .graph-container .graph ul.date-list.tue > li:first-child:not(.Jan_01):not(.Jan_02):not(.Jan_03),
  .header-section .graph-container .graph ul.date-list.wed > li:first-child:not(.Jan_01):not(.Jan_02):not(.Jan_03):not(.Jan_04),
  .header-section .graph-container .graph ul.date-list.thu > li:first-child:not(.Jan_01):not(.Jan_02):not(.Jan_03):not(.Jan_04):not(.Jan_05),
  .header-section .graph-container .graph ul.date-list.fri > li:first-child:not(.Jan_01):not(.Jan_02):not(.Jan_03):not(.Jan_04):not(.Jan_05):not(.Jan_06),
  .header-section .graph-container .graph ul.date-list.sat > li:first-child:not(.Jan_01):not(.Jan_02):not(.Jan_03):not(.Jan_04):not(.Jan_05):not(.Jan_06):not(.Jan_07) {
    margin-left: 16px;
  }

  .header-section .graph-container .graph ul.date-list.sun > li::before {
    font-size: 12px;
    display: block;
    position: absolute;
    top: -16px;
    left: 0;
  }

  .header-section .graph-container .graph ul.date-list.sun > :not(li[class^="Feb"]) + li[class^="Feb"]::before {
    content: 'Feb';
  }

  .header-section .graph-container .graph ul.date-list.sun > :not(li[class^="Mar"]) + li[class^="Mar"]::before {
    content: 'Mar';
  }

  .header-section .graph-container .graph ul.date-list.sun > :not(li[class^="Apr"]) + li[class^="Apr"]::before {
    content: 'Apr';
  }

  .header-section .graph-container .graph ul.date-list.sun > :not(li[class^="May"]) + li[class^="May"]::before {
    content: 'May';
  }

  .header-section .graph-container .graph ul.date-list.sun > :not(li[class^="Jun"]) + li[class^="Jun"]::before {
    content: 'Jun';
  }

  .header-section .graph-container .graph ul.date-list.sun > :not(li[class^="Jul"]) + li[class^="Jul"]::before {
    content: 'Jul';
  }

  .header-section .graph-container .graph ul.date-list.sun > :not(li[class^="Aug"]) + li[class^="Aug"]::before {
    content: 'Aug';
  }

  .header-section .graph-container .graph ul.date-list.sun > :not(li[class^="Sep"]) + li[class^="Sep"]::before {
    content: 'Sep';
  }

  .header-section .graph-container .graph ul.date-list.sun > :not(li[class^="Oct"]) + li[class^="Oct"]::before {
    content: 'Oct';
  }

  .header-section .graph-container .graph ul.date-list.sun > :not(li[class^="Nov"]) + li[class^="Nov"]::before {
    content: 'Nov';
  }

  .header-section .graph-container .graph ul.date-list.sun > :not(li[class^="Dec"]) + li[class^="Dec"]::before {
    content: 'Dec';
  }

  .header-section .graph-container .graph .side-header {
    margin-right: 6px;
  }

  .header-section .graph-container .graph .top-header {
    height: 26px;
  }

  .header-section .graph-container .graph ul li {
    line-height: 16px;
  }

  .header-section .graph-container .graph ul.date-list li{
    margin: 0 2px;
  }

  .header-section .graph-container .graph ul li p {
    line-height: 16px;
  }

  .header-section .graph-container .graph button {
    background-color: var(--lot-activity-color);
    border: none;
    width: 10px;
    height: 10px;
    border-radius: 2px;
    padding: 0;
    cursor: pointer;
  }

  .header-section .graph-container .graph button:hover {
    transform: scale(1.2);
  }

  .header-section .graph-container .graph button[class$="-0"] {
    background-color: var(--inactivity-color);
  }

  .header-section .graph-container .graph button[class$="-1"] {
    background-color: var(--one-activity-color);
  }

  .header-section .graph-container .graph button[class$="-2"] {
    background-color: var(--two-activity-color);
  }

  .header-section .graph-container .graph button[class$="-3"] {
    background-color: var(--three-activity-color);
  }

  .header-section .graph-container .graph button[class$="-2"] {
    background-color: var(--two-activity-color);
  }

  .header-section .graph-container .bottom-graph {
    position: absolute;
    bottom: 6px;
    display: flex;
    justify-content: space-between;
    width: 95%;
  }

  .header-section .graph-container .bottom-graph p {
    color: #768390;
  }

  .header-section .graph-container .bottom-graph .legend-graph,
  .header-section .graph-container .bottom-graph .legend-graph ul {
    display: flex;
    align-items: center;
  }

  .header-section .graph-container .bottom-graph .legend-graph ul {
    width: 68px;
    justify-content: space-between;
    margin: 0 6px;
  }

  .header-section .graph-container .bottom-graph .legend-graph ul li {
    width: 10px;
    height: 10px;
    background-color: #444c56;
    border-radius: 2px;
  }

  .header-section .graph-container .bottom-graph .legend-graph ul li:first-child {
    background-color: var(--inactivity-color);
  }

  .header-section .graph-container .bottom-graph .legend-graph ul li:nth-child(2) {
    background-color: var(--one-activity-color);
  }

  .header-section .graph-container .bottom-graph .legend-graph ul li:nth-child(3) {
    background-color: var(--two-activity-color);
  }

  .header-section .graph-container .bottom-graph .legend-graph ul li:nth-child(4) {
    background-color: var(--three-activity-color);
  }

  .header-section .graph-container .bottom-graph .legend-graph ul li:nth-child(5) {
    background-color: var(--lot-activity-color);
  }
</style>
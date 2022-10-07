<template>
  <section class="contribution-timeline">
    <p>Contribution activity</p>
    <div class="top-section">
      <p class="selected-date">{{formatDate() || selectedYear}}</p>
      <div class="line"></div>
    </div>
    <p v-if="!contributions.length" class="no-activity-text">No activity during this period.</p>
    <a-timeline>
      <a-timeline-item color="gray" v-for="contr of contributions" :key="contr">
        <p class="title">Create {{contr.type}} in 1 repository</p>
        <div class="contribution-item">
          <a :href="contr.url">{{contr.actor}}/{{contr.repository_name}} <span>1 {{contr.type}}</span></a>
        </div>
      </a-timeline-item>
    </a-timeline>
  </section>
</template>

<script>
import { format } from 'date-fns';

  export default {
    name: 'ContributionActivity',
    props: {
      contributions: Array,
      selectedDate: Date,
      selectedYear: Number,
    },
    methods: {
      formatDate() {
        console.log("🚀 ~ file: ContributionActivity.vue ~ line 32 ~ formatDate ~ this.selectDate", this.selectedDate)
        return this.selectedDate ? format(this.selectedDate, 'LLLL d, yyyy') : null;
      }
    }
  }
</script>

<style>
  .contribution-timeline > p {
    margin-bottom: 12px;
  }

  .contribution-timeline .ant-timeline {
    margin-top: 22px;
  }

  .contribution-timeline .selected-date {
    font-size: 12px;
  }

  .contribution-timeline .top-section {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .contribution-timeline .top-section .line {
    width: 85%;
    border-top: 1px solid var(--font-primary);
  }

  .contribution-timeline .ant-timeline-item-head-gray {
    color: var(--inactivity-color);
    border-color: var(--inactivity-color);
  }

  .contribution-timeline .ant-timeline-item-head {
    width: 20px;
    height: 20px;
    background-color: var(--inactivity-color);
    border: 2px solid var(--font-primary);
    border-radius: 100px;
  }

  .contribution-timeline .ant-timeline-item-content {
    padding-top: 4px;
  }

  .contribution-timeline .ant-timeline-item-content p {
    color: var(--font-primary);
  }

  .contribution-timeline .ant-timeline-item-content p:not(.title) {
    font-size: 12px;
  }

  .contribution-timeline .ant-timeline-item .ant-timeline-item-tail {
    left: 9px;
  }

  .contribution-timeline .ant-timeline-item .contribution-item span {
    color: var(--font-secondary);
    font-size: 11px;
  }

  .contribution-timeline .no-activity-text {
    text-align: center;
    padding: 22px;
  }
</style>

<template>
  <section class="date-filters flex flex-col md:flex-row items-start md:items-center">
    <div class="flex flex-col md:flex-row space-y-2 md:space-y-0 md:space-x-2">
      <label class="mr-2">Joined Twitter between:</label>
         <Datepicker v-model="localStartDate" placeholder="Select From Date" class="w-full md:w-auto" />
        <Datepicker v-model="localEndDate" placeholder="Select To Date" class="w-full md:w-auto" />
    </div>
  </section>
</template>

<script>
import Datepicker from 'vue3-datepicker';

export default {
  components: { Datepicker },
  props: ['startDate', 'endDate'],
  emits: ['updateStartDate', 'updateEndDate'],
  data() {
    return {
      localStartDate: this.startDate,
      localEndDate: this.endDate,
    };
  },
  watch: {
    localStartDate(newValue) {
        this.$emit('updateStartDate', newValue);
    },
    localEndDate(newValue) {
        this.$emit('updateEndDate', newValue);
    },
    startDate(newValue) {
       this.localStartDate = newValue;
    },
    endDate(newValue) {
      
      this.localEndDate = newValue;
    },
  },
};
</script>

<style scoped>
.date-filters {
  margin-bottom: 16px;
}
</style>

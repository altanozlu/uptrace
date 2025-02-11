<template>
  <div class="text-no-wrap">
    <v-btn
      text
      icon
      :disabled="!dateRange.hasPrevPeriod"
      title="Previous period"
      @click="dateRange.prevPeriod"
    >
      <v-icon class="small">mdi-chevron-left</v-icon>
    </v-btn>

    <DateTimePeriodMenu v-if="dateRange.gte" :date-range="dateRange" :periods="periods" />
    <PeriodPickerMenu
      :value="dateRange.duration"
      :periods="periods"
      @input="dateRange.changeDuration($event)"
    />

    <v-btn
      text
      icon
      :disabled="!dateRange.hasNextPeriod"
      title="Next period"
      @click="dateRange.nextPeriod"
    >
      <v-icon class="small">mdi-chevron-right</v-icon>
    </v-btn>

    <v-btn small outlined class="ml-2" @click="dateRange.reload">
      <v-icon small class="mr-1">mdi-refresh</v-icon>
      <span>{{ dateRange.isNow ? 'Reload' : 'Reset' }}</span>
    </v-btn>
  </div>
</template>

<script lang="ts">
import { defineComponent, computed, watchEffect, onMounted, PropType } from '@vue/composition-api'

// Composables
import { UseDateRange } from '@/use/date-range'

// Components
import DateTimePeriodMenu from '@/components/DateTimePeriodMenu.vue'
import PeriodPickerMenu from '@/components/PeriodPickerMenu.vue'

// Utilities
import { hour } from '@/util/date'
import { periodsForDays } from '@/models/period'

export default defineComponent({
  name: 'DateRangePicker',
  components: { DateTimePeriodMenu, PeriodPickerMenu },

  props: {
    dateRange: {
      type: Object as PropType<UseDateRange>,
      required: true,
    },
    defaultPeriod: {
      type: Number,
      default: hour,
    },
  },

  setup(props) {
    const periods = computed(() => {
      return periodsForDays(30)
    })

    onMounted(() => {
      watchEffect(() => {
        if (props.dateRange.duration) {
          return
        }

        const period = periods.value.find((p) => p.ms === props.defaultPeriod)
        if (period) {
          props.dateRange.changeDuration(period.ms)
          return
        }

        props.dateRange.changeDuration(periods.value[0].ms)
      })
    })

    return { periods }
  },
})
</script>

<style lang="scss" scoped></style>

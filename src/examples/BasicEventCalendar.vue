<template>
  <DayPilotCalendar viewType="Week" :startDate="startDate" :events="events">
    <template #event="{event}">
      {{event.text()}} {{event.start().toString("h:mm tt")}}
    </template>
  </DayPilotCalendar>
</template>

<script setup>
import {DayPilotCalendar} from '@daypilot/daypilot-lite-vue';

defineProps({
  startDate: {type: [String, Object], required: true},
  events: {type: Array, default: () => []},
});
</script>

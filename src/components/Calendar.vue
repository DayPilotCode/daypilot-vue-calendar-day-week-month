<template>
  <div style="display: flex">
    <div style="margin-right: 10px">
      <DayPilotNavigator
          :selectMode="viewType"
          :showMonths="3"
          :skipMonths="3"
          @timeRangeSelected="args => startDate = args.day">
      </DayPilotNavigator>
    </div>
    <div style="flex-grow: 1;">
      <div class="buttons">
        <button @click="viewType='Day'" :class="{ selected: viewType === 'Day' }">Day</button>
        <button @click="viewType='Week'" :class="{ selected: viewType === 'Week' }">Week</button>
        <button @click="viewType='Month'" :class="{ selected: viewType === 'Month' }">Month</button>
      </div>
      <DayPilotCalendar
          :viewType="'Day'"
          :startDate="startDate"
          :visible="viewType === 'Day'"
          :events="events"
          @beforeEventRender="onBeforeEventRender"
          @timeRangeSelected="onTimeRangeSelected"
          ref="dayRef"
      >
        <template #event="{event}">
          <CalendarEvent
              :event="event"
              @edit="onEventEdit"
              @delete="onEventDelete"
          />
        </template>
      </DayPilotCalendar>
      <DayPilotCalendar
          :viewType="'Week'"
          :startDate="startDate"
          :visible="viewType === 'Week'"
          :events="events"
          :eventBorderRadius="5"
          :durationBarVisible="false"
          @beforeEventRender="onBeforeEventRender"
          @timeRangeSelected="onTimeRangeSelected"
          ref="weekRef"
      >
        <template #event="{event}">
          <CalendarEvent
              :event="event"
              @edit="onEventEdit"
              @delete="onEventDelete"
          />
        </template>
      </DayPilotCalendar>
      <DayPilotMonth
          :startDate="startDate"
          :visible="viewType === 'Month'"
          :events="events"
          @beforeEventRender="onBeforeEventRender"
          @timeRangeSelected="onTimeRangeSelected"
          ref="monthRef"
      >
        <template #event="{event}">
          <CalendarEvent
              :event="event"
              :use-header="false"
              @edit="onEventEdit"
              @delete="onEventDelete"
          />
        </template>
      </DayPilotMonth>
    </div>
  </div>

</template>

<script setup>
import {DayPilot, DayPilotCalendar, DayPilotMonth, DayPilotNavigator} from '@daypilot/daypilot-lite-vue';
import {ref, onMounted} from 'vue';
import CalendarEvent from "@/components/CalendarEvent.vue";

const events = ref([]);
const viewType = ref("Week");
const startDate = ref(DayPilot.Date.today());

const dayRef = ref(null);
const weekRef = ref(null);
const monthRef = ref(null);

const onTimeRangeSelected = async (args) => {
  const modal = await DayPilot.Modal.prompt("Create a new event:", "Event 1");
  const calendar = args.control;
  calendar.clearSelection();
  if (modal.canceled) { return; }
  calendar.events.add({
    start: args.start,
    end: args.end,
    id: DayPilot.guid(),
    text: modal.result
  });
};

const onBeforeEventRender = (args) => {
  args.data.barHidden = true;
  args.data.borderRadius = "5px";
  args.data.backColor = (args.data.color || "#cccccc") + "cc";
  args.data.borderColor = "darker";
};

const colors = [
  {name: "Green", id: "#93c47d"},
  {name: "Blue", id: "#6fa8dc"},
  {name: "Orange", id: "#f6b26b"},
  {name: "Yellow", id: "#ffd966"},
  {name: "Gray", id: "#cccccc"},
];

const onEventEdit = async  (event) => {
  const form = [
    {name: "Text", id: "text", type: "text"},
    {name: "Color", id: "color", type: "select", options: colors},
  ];
  const modal = await DayPilot.Modal.form(form, event.data);
  if (modal.canceled) {
    return;
  }
  event.data.text = modal.result.text;
  event.data.color = modal.result.color;
};

const onEventDelete = (event) => {
  const data = event.data;
  events.value = events.value.filter(e => e.id !== data.id);
}

const loadEvents = () => {
  const firstDay = DayPilot.Date.today().firstDayOfWeek().addDays(1);
  events.value = [
    { id: 1, start: firstDay.addHours(9), end: firstDay.addHours(10), text: "Event 1", color: "#93c47d"},
    { id: 2, start: firstDay.addDays(1).addHours(11), end: firstDay.addDays(1).addHours(12), text: "Event 2", color: "#cccccc"},
    { id: 3, start: firstDay.addDays(1).addHours(13), end: firstDay.addDays(1).addHours(15), text: "Event 3", color: "#6fa8dc"},
    { id: 4, start: firstDay.addHours(15), end: firstDay.addHours(16), text: "Event 4", color: "#f6b26b"},
    { id: 5, start: firstDay.addHours(11), end: firstDay.addHours(14), text: "Event 5", color: "#ffd966"},
  ];
};

onMounted(() => {
  loadEvents();
});

</script>

<style>
/* override the built-in theme, must not be scoped */
body .calendar_default_event_inner {
  padding: 0px;
}
</style>


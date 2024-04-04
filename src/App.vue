<template>
  <div class="text-center">
    <h1>North Beach Pool Schedule</h1>
    <h2>Spring 2024</h2>
  </div>
  <div class="py-4 px-2">
    <div class="">
      <div
        v-for="day in Object.keys(events)"
        class="mr-4 border-b border-black"
      >
        <span
          :class="{ 'text-orange-500 font-bold': day === currentDay }"
          class="capitalize"
          >{{ day }}</span
        >
        <div v-if="day === currentDay" class="text-sm">
          <h1>
            Currently it is <span class="capitalize">{{ currentDay }}</span
            >,
            {{ currentTime }}
          </h1>
          <small class="text-orange-500"
            >Upcoming blocks are highlighted in orange</small
          >
        </div>

        <div class="flex">
          <div
            v-for="event in events[day]"
            class="border border-gray-400 p-1 my-2 min-w-24 w-24 text-sm mr-2"
            :class="{
              'text-orange-500 border-orange-500':
                isNearTimeRange(event.start, event.end, 24 * 60) &&
                day === currentDay,
            }"
          >
            {{ event.start }} to {{ event.end }}
            <div
              :class="`${event.pool === 1 ? 'text-blue-500' : 'text-red-500'}`"
            >
              {{ pools[event.pool] }} pool
            </div>
          </div>
        </div>
      </div>
    </div>
    <a
      class="text-blue-500 underline"
      href="https://sfrecpark.org/Facilities/Facility/Details/North-Beach-Pool-218"
      >Link to official North Beach Pool Website</a
    >
  </div>
</template>

<script setup>
const pools = ["warm", "cool"];

const events = {
  tuesday: [
    { start: "06:30 AM", end: "08:30 AM", pool: 0 },
    { start: "06:30 AM", end: "08:30 AM", pool: 1 },
    { start: "09:00 AM", end: "10:30 AM", pool: 1 },
    { start: "11:30 AM", end: "02:00 PM", pool: 1 },
    { start: "12:15 PM", end: "02:00 PM", pool: 0 },
    { start: "2:15 PM", end: "2:30 PM", pool: 1 },
  ],
  wednesday: [
    { start: "09:00 AM", end: "10:30 AM", pool: 1 },
    { start: "11:30 AM", end: "12:30 PM", pool: 1 },
    { start: "12:45 PM", end: "02:00 PM", pool: 1 },
    { start: "02:15 PM", end: "03:30 PM", pool: 1 },
    { start: "02:15 PM", end: "03:30 PM", pool: 0 },
  ],
  thursday: [
    { start: "07:30 AM", end: "08:45 AM", pool: 0 },
    { start: "07:30 AM", end: "08:45 AM", pool: 1 },
    { start: "09:00 AM", end: "10:30 AM", pool: 1 },
    { start: "02:15 PM", end: "03:15 PM", pool: 1 },
  ],
  friday: [
    { start: "09:00 AM", end: "10:30 AM", pool: 1 },
    { start: "11:30 AM", end: "12:30 PM", pool: 1 },
    { start: "12:45 PM", end: "02:00 PM", pool: 1 },
    { start: "02:15 PM", end: "03:30 PM", pool: 0 },
    { start: "05:30 PM", end: "07:30 PM", pool: 1 },
  ],
  saturday: [
    { start: "12:00 PM", end: "01:15 PM", pool: 1 },
    { start: "01:45 PM", end: "03:00 PM", pool: 1 },
    { start: "03:30 PM", end: "04:30 PM", pool: 1 },
  ],
};

const currentDate = new Date();
const currentDayNumber = currentDate.getDay();
const daysOfWeek = [
  "sunday",
  "monday",
  "tuesday",
  "wednesday",
  "thursday",
  "friday",
  "saturday",
];
const currentDay = daysOfWeek[currentDayNumber];

const currentTime = new Date().toLocaleTimeString([], {
  hour: "2-digit",
  minute: "2-digit",
});

function isNearTimeRange(startTime, endTime, timeDiffMins = 60) {
  const now = new Date();
  const currentHour = now.getHours();
  const currentMinute = now.getMinutes();
  const isAM = currentHour < 12;

  const currentMinutes = currentHour * 60 + currentMinute;

  const startTimeParts = startTime.split(":");
  const startHour = parseInt(startTimeParts[0]);
  const startMinute = parseInt(startTimeParts[1]);
  const startMinutes = startHour * 60 + startMinute;

  const endTimeParts = endTime.split(":");
  const endHour = parseInt(endTimeParts[0]);
  const endMinute = parseInt(endTimeParts[1]);
  const endMinutes = endHour * 60 + endMinute;
  const endIsAM = endTime.split(" ")[1] === "AM";
  const endIsPM = !endIsAM;

  const endTimeLaterThanCurrentTime =
    (isAM && endIsPM) ||
    currentHour < endHour ||
    (currentHour === endHour && currentMinute < endMinute);

  if (
    endTimeLaterThanCurrentTime &&
    (Math.abs(currentMinutes - startMinutes) <= timeDiffMins ||
      Math.abs(currentMinutes - endMinutes) <= timeDiffMins)
  ) {
    return true;
  } else {
    return false;
  }
}
</script>

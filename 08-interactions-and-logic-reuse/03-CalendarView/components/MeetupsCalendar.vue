<template>
  <calendar-view v-slot="{ day }">
    <!-- Каждый митап - ссылка на страницу митапа -->
    <!-- Используя слот требуется вывести список митапов дня в каждой ячейке -->
    <router-link
      v-for="meetup in meetupDays[day]"
      :key="meetup.id"
      :to="{ name: 'meetup', params: { meetupId: meetup.id } }"
      class="rangepicker__event"
      >{{ meetup.title }}</router-link
    >
  </calendar-view>
</template>

<script>
import CalendarView from './CalendarView';

export default {
  name: 'MeetupsCalendar',

  props: {
    meetups: {
      type: Array,
      required: true,
    },
  },

  components: {
    CalendarView,
  },
  computed: {
    meetupDays() {
      const meetupDay = {};
      this.meetups.map((item) => {
        const eventDay = new Date(item.date).toDateString();
        if (meetupDay[eventDay]) {
          meetupDay[eventDay].push({ title: item.title, id: item.id });
        } else {
          meetupDay[eventDay] = [{ title: item.title, id: item.id }];
        }
         /*meetupDay[eventDay] = meetupDay[eventDay]
          ? meetupDay[eventDay].push({ title: item.title, id: item.id })
          : [{ title: item.title, id: item.id }];*/
      });
      return meetupDay;
    },
  },
};
</script>

<style scoped>
.rangepicker__event {
  --maxLines: 2;
  --lineHeight: 16px;
  --fontSize: 14px;

  display: block;

  text-align: left;
  text-overflow: ellipsis;
  overflow: hidden;
  font-size: var(--fontSize);
  line-height: var(--lineHeight);
  font-weight: 600;
  color: var(--white);
  padding: 4px 6px;
  border-radius: 2px;
  background-color: var(--blue);
  text-decoration: none;
  margin-top: 4px;
}

@media all and (min-width: 767px) {
  .rangepicker__event {
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    max-height: calc(var(--maxLines) * var(--lineHeight) + 6px);
  }
}
</style>

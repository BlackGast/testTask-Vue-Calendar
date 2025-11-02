<template>
  <div>
    <div style="margin-bottom: 10px;">
      <button @click="locale = 'en'">English</button>
      <button @click="locale = 'ru'">Русский</button>
    </div>
  </div>
  <Calendar :initialDate="''" :locale="locale" @select="onDateSelect" />
</template>

<script>
import Calendar from './components/Calendar.vue';

export default {
  name: 'App',
  components: {
    Calendar
  },
  data() {
    return {
      locale: 'ru'
    }
  },
  methods: {
    onDateSelect(date) {
      console.log(this.locale === 'ru' ? 'Выбрана дата: ' : 'Selected date: ', date);
    }
  }
}
</script>

<style scoped></style>
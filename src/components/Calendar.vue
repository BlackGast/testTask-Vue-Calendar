<template>
    <div class="calendar">
        <div class="calendar-header">
            <button @click="prevMonth">
                &lt;</button>
            <span style="font-size: 12px;">{{ monthName }} {{ currentYear }}</span>
            <button @click="nextMonth">&gt;</button>
        </div>

        <div class="calendar-weekdays">
            <div v-for="day in weekdays" :key="day" class="weekday">
                {{ day }}</div>
        </div>

        <div class="calendar-days">
            <div v-for="day in daysInMonth" :key="day.date" class="day" :class="{
                today: day.isToday,
                selected:
                    day.isSelected
            }" @click="selectDate(day.date)">
                {{ day.number }}
            </div>
        </div>
    </div>
</template>

<script>


export default {
    name: 'Calendar',
    props: {
        initialDate: {
            type: String,
            default: ''
        },
        locale: {
            type: String,
            default: 'ru'
        }
    },
    data() {
        const today = new Date();
        return {
            today,
            selectedDate: null,
            currentMonth: today.getMonth(),
            currentYear: today.getFullYear(),
            locales: {
                ru: {
                    months: [
                        'Январь', 'Февраль', 'Март', 'Апрель', 'Май', 'Июнь',
                        'Июль', 'Август', 'Сентябрь', 'Октябрь', 'Ноябрь', 'Декабрь'
                    ],
                    weekdays: ['Пн', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб', 'Вс']
                },
                en: {
                    months: [
                        'January', 'February', 'March', 'April', 'May', 'June',
                        'July', 'August', 'September', 'October', 'November', 'December'
                    ],
                    weekdays: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
                }
            }
        }
    },
    computed: {
        monthName() {
            return this.locales[this.locale].months[this.currentMonth]
        },
        weekdays() {
            return this.locales[this.locale].weekdays;
        },
        daysInMonth() {
            const days = [];
            const firstDay = new Date(this.currentYear, this.currentMonth, 1);
            const lastDay = new Date(this.currentYear, this.currentMonth + 1, 0);
            for (let day = 1; day <= lastDay.getDate(); day++) {
                const date = new Date(this.currentYear, this.currentMonth, day);
                const isoDate = date.getFullYear() + '-' +
                    String(date.getMonth() + 1).padStart(2, '0') + '-' +
                    String(date.getDate()).padStart(2, '0');
                days.push({
                    number: day,
                    date: isoDate,
                    isToday: this.isSameDate(date, this.today),
                    isSelected: this.selectedDate
                        ? this.isSameDate(date, new Date(this.selectedDate))
                        : false
                })
            };
            return days;
        }
    },
    methods: {
        isSameDate(date1, date2) {
            return (
                date1.getFullYear() === date2.getFullYear() &&
                date1.getMonth() === date2.getMonth() &&
                date1.getDate() === date2.getDate()
            );
        },
        selectDate(date) {
            this.selectedDate = date;
            this.$emit('select', date);
        },
        prevMonth() {
            if (this.currentMonth === 0) {
                this.currentMonth = 11;
                this.currentYear--;
            } else {
                this.currentMonth--;
            }
        },
        nextMonth() {
            if (this.currentMonth === 11) {
                this.currentMonth = 0;
                this.currentYear++;
            } else {
                this.currentMonth++;
            }
        }
    },
    mounted() {
        if (this.initialDate) {
            const date = new Date(this.initialDate);
            if (!isNaN(date)) {
                this.currentMonth = date.getMonth();
                this.currentYear = date.getFullYear();
                this.selectedDate = this.initialDate;
            }
        }
    }
}
</script>

<style scoped>
.calendar {
    width: 220px;
    border: 1 px solid #dddddd;
    border-radius: 4px;
    padding: 8px;
    box-sizing: border-box;
    font-family: Arial, sans-serif;
    font-size: 13px;
    color: #333333;
    background: #ffffff;
}

.calendar-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 6px;
    user-select: none;
}

.calendar-weekdays,
.calendar-days {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    /* gap: 5px; */
    text-align: center;
}

/* .weekday,
.day {
    text-align: center;
    padding: 5px 0;
} */

.weekday {
    /* font-weight: bold; */
    font-size: 9px;
    color: #000000;
    margin-bottom: 4px;
}

.day {
    cursor: pointer;
    border-radius: 3px;
    padding: 4px 0;
    line-height: 20px;
}

.day:hover {
    background-color: #f2f2f2;
}

.day.today {
    border: 1px solid #007bff;
    border-radius: 3px;
}

.day.selected {
    background-color: #007bff;
    color: white;
}
</style>
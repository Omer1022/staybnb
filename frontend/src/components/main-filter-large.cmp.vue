<template>
    <form class="header-filter">
        <div class="destination-input btn-container">
            <button @click.prevent="clickedButton">
                <label>
                    <div class="button-title">Where</div>
                    <input v-model="filterBy.destination" name="destination-input" type="text"
                        placeholder="Search detonations" />
                </label>
            </button>
        </div>

    <div class="date-container">
      <div class="btn-container flex">
        <button @click.prevent="isCalendarShown = !isCalendarShown">
          <div class="button-title">Check in</div>
          <p class="button-sub">{{ checkInDate }}</p>
        </button>
      </div>

      <div class="btn-container flex">
        <button @click.prevent="isCalendarShown = !isCalendarShown">
          <div class="button-title">Check out</div>
          <p class="button-sub">{{ checkOutDate }}</p>
        </button>
      </div>
    </div>

    <span>|</span>

            <div class="btn-container flex">
                <button @click.prevent="isCalendarShown = !isCalendarShown">
                    <div class="button-title">Check out</div>
                    {{ checkOutDate }}
                </button>
            </div>
        

        <span>|</span>

        <div class="calendar-modal" :class="{ 'active-calendar': isCalendarShown }">
            <calender-spread @closeCalendar="isCalendarShown = false" @dateChange="dateUpdate" @click.prevent
                is-expanded>
            </calender-spread>
        </div>

        <div class="guest-container  btn-container flex">
            <button @click.prevent="isGuestModalShown = !isGuestModalShown">
                <div class="button-title">Who</div>
                <span class="guests-sum">{{ totalGuests }}</span>
            </button>
            <div @click.prevent="runSearch" class="filter-search">
                <img src="../styles/icons/search_white.png" alt="" /> <span>Search</span>
            </div>
            <div class="guests-modal" :class="{ 'active-guest': isGuestModalShown }">
                <guests-picker @guestsUpdate="updateGuests" @closeGuestsModal="isGuestModalShown = false" />
            </div>
        </div>
    </form>
</template>

<script>
import guestsPicker from "./guests-picker.cmp.vue";
import calenderSpread from "./calender-spread.vue";


export default {
    props: {
        mode: String
    },
    components: {
        guestsPicker,
        calenderSpread,
    },
    data() {
        return {
            filterBy: {
                destination: "",
                numOfBeds: 0,
            },
            date: {
                start: null,
                end: null,
            },
            guests: {
                adults: 0,
                kids: 0,
                infants: 0,
                total: 0,
            },

            isCalendarShown: false,
            isGuestModalShown: false,
            mode: 'destination',
        };
    },
    methods: {
        clickedButton() {
            console.log('clicked')
        },
        runSearch() {
            if (!this.filterBy.destination.length) return
            if (this.guests.total === 0) this.filterBy.numOfBeds = 1
            const copyFilter = JSON.parse(JSON.stringify(this.filterBy))
            this.$store.dispatch({ type: "setFilter", filterBy: copyFilter })
        },

        dateUpdate(newDate) {
            this.date = newDate
        },
        updateGuests(NewGuests) {
            this.guests = NewGuests
            this.filterBy.numOfBeds = this.guests.total
        },
    },
    actions: {},
    computed: {
        checkInDate() {
            return this.date.start ? this.date.start.toLocaleDateString('en-US', {
                year: 'numeric',
                month: '2-digit',
                day: '2-digit',
            }) : "Add dates"
        },

        checkOutDate() {
            return this.date.end ? this.date.end.toLocaleDateString('en-US', {
                year: 'numeric',
                month: '2-digit',
                day: '2-digit',
            }) : "Add dates"
        },
        totalGuests() {
            switch (this.guests.total) {
                case 0:
                    return 'Add guests'
                    break
                case 1:
                    return '1 guest'
                    break
                default:
                    return `${this.guests.total} guests`
            }
        },
    },
    watch: {
        // guests(){
        //     console.log(this.guests.total)
        //     this.filterBy.numOfBeds = this.guests.total 
        // }
    },
    created() {

    },
    unmounted() { },
};
</script>

<template>
  <section class="details-modal">
    <div class="stay-order">
      <div class="order-modal-container">
        <div class="order-modal flex">
          <div class="mini-modal-container">
            <div class="order-form-header flex">
              <div>
                <span class="stay-details-price bold">${{ stay.price }}</span>
                night
              </div>
              <div class="reviews-preview flex">
                <div class="star-preview">
                  <img src="../styles/icons/star.svg" class="star" />
                </div>
                <span class="review-avg"> 4.7</span> ·
                <span class="total-reviews">3 Reviews</span>
              </div>
            </div>
          </div>
          <form class="order-form">
            <div class="dates-pick flex">
              <div class="date-picker-container-left">
                <button
                  class="check-in"
                  @click.stop="isCalendarShown = !isCalendarShown"
                >
                  <div class="order-button">CHECK-IN</div>
                  <span class="calender-pick">{{ checkInDate }}</span>
                </button>
              </div>
              <div class="date-picker-container-right">
                <button
                  class="check-out"
                  @click.stop="isCalendarShown = !isCalendarShown"
                >
                  <div class="order-button">CHECKOUT</div>
                  <span class="calender-pick">{{ checkOutDate }}</span>
                </button>
              </div>
            </div>
            <div class="guest-input">
              <button
                @click.stop="isGuestModalShown = !isGuestModalShown"
                class="guests"
              >
                <label class="order-button">GUESTS</label>
                <span class="guest-num">{{ totalGuests }}</span>
              </button>
            </div>
            <fancy-btn class="reserve-btn">Reserve</fancy-btn>
          </form>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import { stayService } from "../services/stay-service";
import calenderSpread from "../components/calender-spread.vue";
import guestsPicker from "../components/guests-picker.cmp.vue";
import fancyBtn from "../components/fancy-btn.cmp.vue";
export default {
  props: [],
  components: {
    guestsPicker,
    calenderSpread,
    fancyBtn,
  },
  data() {
    return {
      stay: null,
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
    };
  },
  methods: {
    removeStay() {
      this.$store
        .dispatch({ type: "removeStay", stayId: this.stay._id })
        .then(() => {
          this.$router.push("/stay");
        });
    },
    sumGuests() {
      this.guests.total =
        this.guests.adults + this.guests.kids + this.guests.infants;
    },
    updateAdults(num) {
      console.log(num);
      this.guests.adults = num;
      this.sumGuests();
    },
    updateKids(num) {
      this.guests.kids = num;
      this.sumGuests();
    },
    updateInfants(num) {
      this.guests.infants = num;
      this.sumGuests();
    },
    dateUpdate(date) {
      this.date = date;
      console.log("yes!");
      console.log("yes", this.date);
      console.log("start", this.date.start);
      console.log("end", this.date.end);
    },
  },
  computed: {
    checkInDate() {
      return this.date.start
        ? this.date.start.toLocaleDateString("en-US", {
            year: "numeric",
            month: "2-digit",
            day: "2-digit",
          })
        : "Add date";
    },

    checkOutDate() {
      return this.date.end
        ? this.date.end.toLocaleDateString("en-US", {
            year: "numeric",
            month: "2-digit",
            day: "2-digit",
          })
        : "Add date";
    },
    totalGuests() {
      return this.guests.total > 0 ? this.guests.total : "1 guest";
    },
    totalGuests() {
      return this.guests.total > 0 ? this.guests.total : "1 guest";
    },
  },
};
</script>

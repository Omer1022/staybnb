<template>
  <div class="header-search">
    <button value="destination" @click="openSearch('destination')">Anywhere</button>
    <span class="header-separator">|</span>
    <button @click="openSearch('date')">Any week</button>
    <span class="header-separator">|</span>

    <div class="add-container flex">
      <button value="guests" class="search-header flex add-btn" @click="openSearch('guests')">
        Add guests
      </button>
     
        <span class="filter-search-icon">
          <img src="../styles/icons/search_white.png" alt="" />
        </span>
     
    </div>
  </div>
</template>
<script>
export default {
  name:'filter-small',
  props: {},
  emits: ['openLarge'],
  data() {
    return {};
  },
  created() { },
  methods: {
    openSearch(mode) {
      console.log('mode', mode)
      this.$emit('openLarge', mode)
    }
  },
  computed: {},
  unmounted() { },
  components: {}
};

</script>

<template>
  <q-item-section class="text-capitalize" v-html='boldenedName'></q-item-section>
</template>
<script>
export default {
  props: ['name', 'search'],
  filters: {
    bolden(value, search) {
      return '<span>' + value.replace(new RegExp(search, "gi"), `<strong>${search}</strong>`) + '</span>';
    }
  },
  computed: {
    boldenedName() {
      return this.$options.filters.bolden(this.name, this.search);
    }
  }
}
</script>
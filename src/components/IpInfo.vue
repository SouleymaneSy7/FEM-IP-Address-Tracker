<template>
  <section class="ip-info">
    <!-- Info Card 1 -->
    <div class="ip__card">
      <h2 class="third-title">IP Address</h2>

      <h3 class="secondary-title">{{ ipDetails.IpAddress }}</h3>
    </div>

    <!-- Info Card 2 -->
    <div class="ip__card">
      <h2 class="third-title">Location</h2>

      <h3 class="secondary-title">{{ ipDetails.city }}</h3>
    </div>

    <!-- Info Card 3 -->
    <div class="ip__card">
      <h2 class="third-title">Timezone</h2>

      <h3 class="secondary-title">UTC{{ ipDetails.timeZone }}</h3>
    </div>

    <!-- Info Card 4 -->
    <div class="ip__card">
      <h2 class="third-title">ISP</h2>

      <h3 class="secondary-title">{{ ipDetails.isp }}</h3>
    </div>
  </section>
</template>

<script setup>
const props = defineProps({
  ipDetails: {
    type: Object,
    required: true,
  },
});
</script>

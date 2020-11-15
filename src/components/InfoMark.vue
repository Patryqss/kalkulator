<template>
  <span>
    <img
      src="icons/question_mark.svg"
      class="icon"
      @mouseenter="showDialog = true"
      @mouseleave="showDialog = false"
    />
    <q-card v-if="showDialog" class="description">
      {{ description }}
    </q-card>
  </span>
</template>

<script>
export default {
  name: 'InfoMark',
  props: {
    description: {
      required: true,
      type: String,
    },
  },
  data() {
    return {
      showDialog: false,
    };
  },
};
</script>

<style lang="scss" scoped>
.description {
  position: absolute;
  z-index: 1;
  padding: 10px;
  font-size: 12px;
}
.icon {
  opacity: 0.5;
  size: 50%;
}
</style>

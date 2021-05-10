<template>
  <article class="panel is-primary">
    <p class="panel-heading py-4">{{ panelDetails.name }}</p>
    <div class="panel-block">
      <p>{{ panelDetails.description }}</p>
    </div>
  </article>
</template>

<script>
export default {
  name: 'PanelHeading',
  props: {
    panelDetails: {
      type: Object,
      default: Object,
    },
  },
}
</script>

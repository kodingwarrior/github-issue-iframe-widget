<template>
  <div @click='toggle'>
    {{ issue.title }}
    {{ issue.body }}
  </div>
</template>

<script>
export default {
  props: ['issue'],
  methods: {
    toggle: function() {
      this.focused = !this.focused;
    }
  },
  data: function () {
    return { 
      focused: false
    }
  }
}
</script>
<style>
</style>


<template>
  <div class='issue'>
    <div :class='{"labels-container": issue.labels.length > 0}'>
      <template v-if='issue.labels' v-for='label in issue.labels'>
        <label-entry :key='label.index' :label='label'/>
      </template>
    </div>
    <h1 @click='toggle'>{{ issue.title }}</h1>
    <div class='detail' v-if='focused'>
      <hr/>
      <vue-markdown class="description">
        {{ issue.body }}
      </vue-markdown>
    </div>
  </div>
</template>

<script>
import VueMarkdown from 'vue-markdown';

import LabelEntry from '@/components/LabelEntry';

export default {
  props: ['issue'],
  components: {
    VueMarkdown,
    LabelEntry
  },
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
<style scoped>
.issue {
  background-color: white;
  border: 1px #DDDFE2 solid;
  border-radius: 10px;

  margin-top: 10px;
  margin-bottom: 10px;
  
  padding: 20px;
}

.issue .detail {
  margin-top: 20px;
}

.issue .detail .description {
  margin-top: 10px;
  line-height: 24px;
  word-break: keep-all;
}

.description 
pre code,
img {
  width: 100%;
  display: block;
  background-color: #EEEFE2;

  overflow-x: scroll;
  padding: 20px 0px;
}

.labels-container {
  margin-bottom: 10px;
}

</style>
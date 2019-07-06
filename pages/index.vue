<template>
  <section class="container">
    <template v-for='issue in issues'>
      <issue-entry v-bind:key="issue.number" :issue="issue"></issue-entry>
    </template>
  </section>
</template>

<script>
import AppLogo from '@/components/AppLogo.vue'
import IssueEntry from '@/components/IssueEntry.vue'
import axios from 'axios'

// from https://github.com/azu/github-issue-widget/blob/gh-pages/src/js/script.js 
function parse_params() {
  var vars = [], hash;
  var hashes = location.href.slice(location.href.indexOf('?') + 1).split('&');
  for (var i = 0, length = hashes.length; i < length; i++) {
    hash = hashes[i].split('=');
    vars.push(hash[0]);

    vars[hash[0]] = hash[1] || true;
  }

  if (typeof vars["state"] === "undefined") {
      vars["state"] = "open";
  }
  // default : limit is `"1"`
  if (typeof vars["limit"] === "undefined") {
      vars["limit"] = "1";
  }
  // default : random is `"false"`
  if (typeof vars["random"] !== "undefined") {
      vars["random"] = true;
  }
  return vars;
}

// http://bost.ocks.org/mike/shuffle/
function shuffle(array) {
    var m = array.length, t, i;
    while (m) {
        i = Math.floor(Math.random() * m--);
        t = array[m];
        array[m] = array[i];
        array[i] = t;
    }
    return array;
}

export default {
  methods: {
    fetchIssues: async function() {
      var APIQueryKeyList = [
        "milestone", "state", "assignee", "creator", "mentioned", "labels", "sort", "direction", "since", "limit"
      ];

      var isAPIQueryKey = function (key) {
        return  APIQueryKeyList.indexOf(key) !== -1;
      };

      var params = parse_params();

      var APIQuery = Object.keys(params)
          .filter(isAPIQueryKey)
          .map(function (key) { return key + "=" + params[key]; });
      
      var APIEndPoint = 'https://api.github.com/repos/' + params.owner + '/' 
                        + params.repo + '/issues' + '?' + APIQuery.join("&");

      var issues = await axios.get(APIEndPoint);
      issues = issues.data
      issues = params["random"] ? shuffle(issues) : issues;

      console.log(issues);
      this.issues = issues;
    }
  },
  components: {
    AppLogo,
    IssueEntry
  },
  data() {
    return {
      issues: []
    }
  },
  mounted() {
    this.fetchIssues()
  }
}
</script>

<style>
body {
  background-color: #E9EBEE;
}

.container {
  padding: 20px;
}
</style>


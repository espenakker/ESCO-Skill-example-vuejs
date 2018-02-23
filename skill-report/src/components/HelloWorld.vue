<template>
  <div>
    <input type="text" v-model="msg" v-on:input="debouncedGet()" />
    <p>Suggestions are:</p> <div id="suggest"></div>
  </div>
</template>

<script>
import axios from 'axios'
import _ from 'lodash'

export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: '',
      suggests: ''
    }
  },
  methods: {
    escoGet: function () {
      const vm = this
      axios.get(`https://ec.europa.eu/esco/api/search?type=skill&language=en&text=` + this.msg)
        .then(
          function (response) {
            console.log(response)
            vm.suggests = ''
            for (var r in response.data._embedded.results) {
              vm.suggests += response.data._embedded.results[r].title + '<br>'
            }
            document.getElementById('suggest').innerHTML = vm.suggests
          }
        )
        .catch(e => {
        })
    },
    debouncedGet: _.debounce(function () { this.escoGet() }, 500)
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
</style>

<template>
  <div id="app">
    <form @submit.prevent="convert">
      <h1>Pegar contentido del csv</h1>
      <p>No pegue la fila que tiene name1, url1</p>
      <label for="columns">Columnas</label>
      <input type="number" id="columns" v-model="columns" />
      <button type="submit">Convertir</button>
    </form>
    <textarea v-model="content" placeholder="Nando,https://url"></textarea>
    <textarea v-model="result"></textarea>
    <hr />
    <h1>Generar campos</h1>
    <button @click="createTemplate">Generar</button>
    <textarea v-model="template"></textarea>
  </div>
</template>

<script>
import chunk from 'lodash/chunk'

const getHeader = columns => {
  const header = []
  for (var i = 1; i <= columns; i++) {
    header.push(`name${i},url${i}`)
  }
  return header.join(',')
}

const template = i => `
<!-- Profile ${i + 1} starts -->
<hr />
<div class="row">
  <div class="col-xs-4 fields">
    <input class="hidden" id="url${i + 1}" name="url${i + 1}" type="hidden" value="\${url${i + 1}}" />
    <input class="hidden" id="name${i + 1}" name="name${i + 1}" type="hidden" value="\${name${i + 1}}" />
    <a class="btn btn-primary" href="\${url${i + 1}}" target="_blank">Open \${name${i + 1}}</a>
  </div>

   <div class="col-xs-4 fields">
      <div class="form-group fieldset">
        <label for="email">Email ${i + 1}:</label>
        <input class="form-control" id="email${i + 1}" name="email${i + 1}" placeholder="e.g. example@email.com" type="email" />
      </div>
   </div>
   <div class="col-xs-4 fields">
      <div class="form-group fieldset">
        <label for="phoneNumber${i + 1}">Phone number ${i + 1}:</label>
        <input class="form-control" id="phoneNumber${i + 1}" name="phoneNumber${i + 1}" placeholder="Enter a phone number" type="tel" />
      </div>
   </div>
</div>

<!-- Profile ${i + 1} ends -->
`

export default {
  name: 'app',
  data: () => ({
    columns: 7,
    fields: 7,
    content: '',
    result: '',
    template: ''
  }),
  methods: {
    convert () {
      const { content, columns } = this
      const rows = content.split('\n')
      let header = getHeader(columns) + '\n'
      const chunks = chunk(rows, columns)
      const result = chunks.map(chunk => chunk.join(',')).join('\n')
      this.result = header + result
    },
    createTemplate () {
      const { columns } = this
      const fields = []
      for (var i = 0; i < columns; i++) {
        fields.push(template(i))
      }
      this.template = fields.join('\n\n\n')
    }
  }
}
</script>

<style scoped>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

img {
  width: 200px;
  height: 200px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>

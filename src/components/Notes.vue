<template>
  <div class="notes">
    
    <h1>{{ title }}</h1>
        <p>Total notes: {{totalNotes}}</p>

        <div class="form">
            <div class="form-group">
                <label>Note title</label>
                <input
                        class="form-control"
                        type="text"
                        v-model="note.title"
                >
            </div>
            <div class="form-group">
                <label>Note text</label>
                <input
                        class="form-control"
                        type="text"
                        v-model="note.text"
                >
            </div>
            <div class="form-group">
                    <label>Email</label>
                    <input
                            class="form-control"
                            type="email"
                            v-model="note.email"
                    >
            </div>
            <button
                    class="btn btn-primary"
                    @click="addNote" 
                    :disabled="isFilledIn"
                    >Submit</button>
        </div>

        <hr />
        <div class="col-sm-12">
            <div class="col-sm-4 note" v-for="(note, index) in notes" v-bind:key="note.index">
                <div class="card">
                    <div class="card-block">
                        <button class="close" @click="removeNote(index)">x</button>
                        <h4 class="card-title">{{ note.title }}</h4>
                        <h6 class="card-subtitle mb-2 text-muted">{{note.date}}</h6>
                        <p class="card-text">{{note.text}}</p>
                        <p class="card-text">{{note.email}}</p>
                    </div>
                </div>
            </div>
        </div>

  </div>
</template>

<script>
export default {
  name: 'Notes',
  data() {
    return {
      title: 'Notes',
      validEmail: /^([a-z0-9_\.-]+)@([a-z0-9_\.-]+)\.([a-z\.]{2,6})$/,
      note: {
        title: '',
        text: '',
        email: ''
      },
      notes: [
        {
          title: 'Visited Hawaii',
          text: 'I loved the beaches and delicious fresh coconuts',
          date: new Date(Date.now()).toLocaleString(),
          email: 'myemail@gmail.com'
        },
        {
          title: 'Visited London',
          text: 'I loved the beaches and delicious fresh coconuts',
          date: new Date(Date.now()).toLocaleString(),
          email: 'myemail@gmail.com'
        }
      ]
    }
  },
  computed: {
    totalNotes() {
      return this.notes.length
    },
    isFilledIn() {
      if (this.validEmail.test(this.note.email)
          && /[^A-Z]/.test(this.note.title)
          && this.note.title.length >= 3
          && this.note.text.split(' ').length >= 3) {
            return false
      } else {
        return true
      }
    }
  },
  methods: {
    addNote(){
      let { text, title, email } = this.note;
      this.notes.push({
        text,
        title,
        date: new Date(Date.now()).toLocaleString(),
        email
      })
      this.note.text = ''
      this.note.title = ''
      this.note.email = ''
    },
    removeNote(index){
      this.notes.splice(index, 1)
    }
    
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
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

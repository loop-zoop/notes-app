<template>
  <div class="notes">
    <h1>{{ title }}</h1>
    <p>Total notes: {{totalNotes}}</p>

    <div class="form">
      <div class="form-group">
        <label>Note title</label>
        <input
          :class="['form-control', isTitleValid ? 'is-valid' : '']"
          type="text"
          v-model="note.title"
          @blur="checkForm"
        >
      </div>
      <div class="form-group">
        <label>Note text</label>
        <input
          :class="['form-control', isTextValid ? 'is-valid' : '']"
          type="text"
          v-model="note.text"
          @blur="checkForm"
        >
      </div>
      <div class="form-group">
        <label>Email</label>
        <input
          :class="['form-control', isEmailValid ? 'is-valid' : '']"
          type="email"
          v-model="note.email"
          @blur="checkForm"
        >
      </div>
      <div v-html="alert"></div>
      <button class="btn btn-primary" @click="addNote" :disabled="isSubmitInactive">Submit</button>
    </div>

    <hr>
    <div class="col-sm-12">
      <p>Sort notes by:</p>
      <div class="btn-group btn-group-toggle" data-toggle="buttons">
        <label :class="['btn', 'btn-primary', { active: isSortedByDate }]">
          <input type="radio" @click="sortByDate" id="bydate" autocomplete="off"> By Date
        </label>
        <label :class="['btn', 'btn-primary', { active: isSortedByTitle }]">
          <input type="radio" @click="sortByTitle" id="bytitle" autocomplete="off"> By Title
        </label>
      </div>
      <div class="note" v-for="(note, index) in notes" :key="note.index">
        <div class="card col-sm-4">
          <div class="card-block">
            <div class="btn-group float-right" role="group">
              <button class="copy" @click="removeNote(index)">&#10006;</button>
              <button class="close" @click="copyNote(index)">&#9998;</button>
            </div>
            <h4 class="card-title">{{ note.title }}</h4>
            <h6 class="card-subtitle mb-2 text-muted">{{note.date.split(',')[0]}}</h6>
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
  name: "Notes",
  data() {
    return {
      title: "Notes",
      isSortedByDate: true,
      isSortedByTitle: false,
      alert: null,
      validEmail: /^([a-z0-9_\.-]+)@([a-z0-9_\.-]+)\.([a-z\.]{2,6})$/,
      note: {
        title: "",
        text: "",
        email: ""
      },
      notes: [
        {
          title: "Visited Amsterdam",
          text: "I loved the beaches and delicious fresh coconuts",
          date: "02.08.2019, 14:13:09",
          email: "myemail@gmail.com"
        },
        {
          title: "Visited Hawaii",
          text: "I loved the beaches and delicious fresh coconuts",
          date: new Date(Date.now()).toLocaleString(),
          email: "myemail@gmail.com"
        },
        {
          title: "Visited London",
          text: "I loved the beaches and delicious fresh coconuts",
          date: new Date(Date.now()).toLocaleString(),
          email: "myemail@gmail.com"
        }
      ]
    };
  },
  computed: {
    totalNotes() {
      return this.notes.length;
    },
    isSubmitInactive() {
      if (this.isTitleValid && this.isTextValid && this.isEmailValid) {
        return false;
      } else {
        return true;
      }
    },
    isTitleValid() {
      if (/^[A-Z]+.*/.test(this.note.title) && this.note.title.length >= 3) {
        return true;
      } else {
        return false;
      }
    },
    isTextValid() {
      if (this.note.text.split(" ").length >= 3) {
        return true;
      } else {
        return false;
      }
    },
    isEmailValid() {
      if (this.validEmail.test(this.note.email)) {
        return true;
      } else {
        return false;
      }
    }
  },
  methods: {
    addNote() {
      let { text, title, email } = this.note;
      this.notes.push({
        text,
        title,
        date: new Date(Date.now()).toLocaleString(),
        email
      });
      this.note.text = "";
      this.note.title = "";
      this.note.email = "";
    },
    removeNote(index) {
      this.notes.splice(index, 1);
    },
    checkForm() {
      let currentAlert = "";
      if (this.note.title !== "" && !this.isTitleValid) {
        currentAlert += `<li>Note Title should be at elast 3 letters long</li>
                        <li>Note Title should start with a capital letter</li>`;
      }
      if (this.note.text !== "" && !this.isTextValid) {
        currentAlert += `<li>Note Text should be at least 3 words long</li>`;
      }
      if (this.note.email !== "" && !this.isEmailValid) {
        currentAlert += `<li>Note Email should be valid</li>`;
      }
      if (currentAlert !== "") {
        this.alert = `
        <div class="alert alert-danger" role="alert">
          <p>Please fix the following:</p>
          <ul>${currentAlert}</ul>
        </div>`;
      } else {
        this.alert = null;
      }
    },
    sortByTitle() {
      this.isSortedByDate = false
      this.isSortedByTitle = true
      this.notes.sort(function(firstEl, secondEl) {
        if(firstEl.title < secondEl.title) { 
          return -1; 
        }
        if(firstEl.title > secondEl.title) { 
          return 1; 
        }
        return 0;
      })
    },
    sortByDate() {
      this.isSortedByDate = true
      this.isSortedByTitle = false
      this.notes.sort(function(firstEl, secondEl) {
        if(firstEl.date < secondEl.date) {
          return 1;
        }
        if(firstEl.date > secondEl.date) { 
          return -1; 
        }
        return 0;
      })
    },
    copyNote(index) {
      this.notes.push(this.notes[index])
    }
  },
  created() {
    this.notes.sort(function(firstEl, secondEl) {
        if(firstEl.date < secondEl.date) {
          return 1;
        }
        if(firstEl.date > secondEl.date) { 
          return -1; 
        }
        return 0;
      })
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
.alert {
  text-align: left;
}
a {
  color: #42b983;
}
.form {
    text-align: left;
}

.card {
    text-align: left;
    border: 1px solid #2c3e50;
    border-radius: 4px;
    padding-left: 8px;
    padding-right: 8px;
}

.note {
    padding: 5px;
}
</style>

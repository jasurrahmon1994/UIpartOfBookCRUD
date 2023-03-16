<template>
  <q-page class="flex flex-center">
    <q-card class="my-card">
      <q-card-section>
        <div class="q-pa-md flex flex-center">
          <q-btn lass="q-ma-lg" label="Add book " color="primary" @click="addBook = true"/>
          <q-item to="/catalogue">
            <q-btn class="q-ma-lg" color="secondary" label="Catalogue"/>
          </q-item>
          <q-dialog v-model="addBook">
            <q-card class="addBookCard">
              <q-card-section>
                <q-form
                  @reset="onReset"
                  class="q-gutter-md"
                  action="http://localhost:9090/library" method="POST"
                >
                  <q-input
                    filled
                    v-model="name"
                    label="Book name *"
                    hint="Full name of the book"
                    name="name"

                    lazy-rules
                    :rules="[ val => val && val.length > 0 || 'Please type name of the book']"
                  />

                  <q-input
                    filled
                    v-model="author"
                    label="Book author *"
                    hint="Full name of the book author"
                    name="author"
                    lazy-rules
                    :rules="[ val => val && val.length > 0 || 'Please type author of the book']"
                  />

                  <q-input
                    filled
                    type="number"
                    v-model="year"
                    name="year"
                    label="Published year *"
                    hint="Type published year of the book"
                    lazy-rules
                    :rules="[
                    val => val !== null && val !== '' || 'Please type published year of the book',
                    val => val > 1700 && val < 2024 || 'Please type published year'
                    ]"
                  />

                  <q-input
                    type="hidden"
                    name="command"
                    class="hidden-input"
                    v-model="command"
                  />

                  <div>
                    <q-btn label="Submit" type="submit" color="primary"/>
                    <q-btn label="Reset" type="reset" color="primary" flat class="q-ml-sm"/>
                  </div>
                </q-form>
              </q-card-section>
            </q-card>
          </q-dialog>
        </div>
      </q-card-section>
    </q-card>

  </q-page>
</template>

<script>
// import {defineComponent} from 'vue'
import {ref} from 'vue'
import { useQuasar } from 'quasar'

export default {
  name: 'IndexPage',
  setup() {
    const name = ref(null)
    const author = ref(null)
    const year = ref(null)

    return {
      addBook: ref(false),
      name,
      author,
      year,
      command: ref('add_book'),

      onReset () {
        name.value = null
        author.value = null
        year.value = null
      }
    }
  }
}

// export default defineComponent({
//   name: 'IndexPage'
// })

</script>
<style lang="scss">
.q-page {
  background: linear-gradient(to bottom, #136a8a, #267871);
}

a {
  text-decoration: none;
  padding: 0 !important;
  margin: 0 !important;
}

.addBookCard {
  width: 100%;
  max-width: 500px
}
.hidden-input {
  display: none;
}
</style>

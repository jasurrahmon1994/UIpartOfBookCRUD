<template>
  <q-page>
    <div class="q-pa-md">
      <q-card class="my-card">
        <q-card-section>
          <div class="text-h6">Books catalogue</div>
          <div class="text-subtitle2">by Jasur Rahmonov</div>
        </q-card-section>

        <q-markup-table>
          <thead>
          <tr>
            <th class="text-left">Name</th>
            <th class="text-right">Author</th>
            <th class="text-right">Published year</th>
            <th class="text-right">Edit</th>
            <th class="text-right">Delete</th>
          </tr>
          </thead>
          <tbody>
          <tr v-for="book in books" :key="book.id">
            <td class="text-left">{{ book.name }}</td>
            <td class="text-right">{{ book.author }}</td>
            <td class="text-right">{{ book.year }}</td>
            <td class="text-right">
              <q-btn label="Update" color="primary" @click="updateBook(book), updateModal=true"/>
            </td>
            <td class="text-right">
              <q-btn label="Delete" color="red" @click="deleteBook(book.id), deleteModal=true"/>
            </td>
          </tr>
          </tbody>
        </q-markup-table>
      </q-card>
    </div>
    <q-dialog v-model="deleteModal" persistent>
      <q-card>
        <q-card-section class="row items-center">
          <q-avatar icon="delete" color="primary" text-color="white"/>
          <span class="q-ml-sm">Do you really want to delete this book?</span>
        </q-card-section>

        <q-card-actions align="right">
          <q-form action="http://localhost:9090/library" method="get">
            <q-input
              type="hidden"
              name="command"
              class="hidden-input"
              v-model="commandDelete"
            />
            <input name="id" v-model="id" class="hidden-input"/>
            <q-btn flat label="Cancel" color="primary" v-close-popup/>
            <q-btn type="submit" flat label="Delete book" color="red" v-close-popup/>
          </q-form>
        </q-card-actions>
      </q-card>
    </q-dialog>
    <q-dialog v-model="updateModal" persistent>
      <q-card>

        <q-card-actions align="right">
          <q-form
            class="q-gutter-md updForm"
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
              v-model="commandUpdate"
            />

            <input name="id" v-model="id" class="hidden-input"/>
            <q-btn flat label="Cancel" color="primary" v-close-popup/>
            <q-btn type="submit" flat label="Update book" color="primary" v-close-popup/>
          </q-form>

        </q-card-actions>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script>
import {ref} from 'vue'
import {api} from 'boot/axios'
export default {
  data() {
    return {
      books: api.get('/library')
        .then((response) => {
          this.books = response.data
        }),
      id: 0,
      name: null,
      author: null,
      year: null
    }
  },
  setup() {
    const name = ref(null)
    const author = ref(null)
    const year = ref(null)
    return {
      deleteModal: ref(false),
      updateModal: ref(false),
      commandDelete: ref('delete_book'),
      commandUpdate: ref('update_book'),
    }
  },
  methods: {
    deleteBook(id){
      this.id = id
    },
    updateBook(book){
      this.name = book.name,
      this.author = book.author,
      this.year = book.year
    }
  }
}
</script>

<style lang="scss">
.hidden-input {
  display: none;
}
.updForm{
  width: 400px;
  padding: 20px;
}
.my-card {
  width: 1000px;
  margin: auto;
  padding: 30px;
}
</style>

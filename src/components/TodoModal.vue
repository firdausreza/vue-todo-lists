<template>
  <section data-cy="modal-add" class="w-full sm:max-w-xl bg-white rounded-lg flex flex-col justify-center items-center">
    <div class="modal-header flex items-center justify-between w-full p-5 border-b">
      <h1 data-cy="modal-add-title" class="text-xl font-bold">{{ mode === 'add' ? 'Tambah' : 'Edit' }} List Item</h1>
      <font-awesome-icon @click="() => $emit('close-modal')" data-cy="modal-add-close-button" icon="fa-solid fa-xmark" class="fa-lg cursor-pointer p-1" />
    </div>
    <div class="modal-body w-full flex flex-col justify-start p-5">
      <div class="input-wrapper flex flex-col justify-start">
        <label data-cy="modal-add-name-title" for="title" class="font-semibold text-sm">NAMA LIST ITEM</label>
        <input data-cy="modal-add-name-input" v-model="todoData.title" type="text" name="title" id="title" class="w-full px-4 py-3 rounded-md mt-2 border border-gray-300" placeholder="Tambahkan nama list item" required>
      </div>
      <div class="input-wrapper flex flex-col justify-start mt-4">
        <label data-cy="modal-add-priority-title" for="priority" class="font-semibold text-sm">PRIORITY</label>
        <select data-cy="modal-add-priority-dropdown" v-model="todoData.priority" name="priority" id="priority" class="w-fit px-4 py-3 mt-2 border border-gray-300 rounded-md">
          <option data-cy="modal-add-priority-item" value="very-high">Very High</option>
          <option value="high">High</option>
          <option value="normal">Medium</option>
          <option value="low">Low</option>
          <option value="very-low">Very Low</option>
        </select>
      </div>
    </div>
    <div class="modal-footer w-full border-t p-5 flex justify-end text-lg font-bold">
      <button data-cy="modal-add-save-button" @click="saveTodo" class="px-12 py-3 bg-blue-500 text-white rounded-full ml-3">
        Simpan
      </button>
    </div>
  </section>

</template>

<script>
import axios from 'axios'
export default {
  name: "TodoModal",
  props: {
    activityId: Number,
    todo: {
      type: Object,
      default: {}
    },
    mode: String
  },
  data() {
    return {
      todoData: {
        title: '',
        priority: 'very-high'
      }
    }
  },
  methods: {
    async saveTodo() {
      if (this.mode === 'edit') {
        if (this.todoData.title === '' || this.todoData.priority === '') {
          console.log('field empty')
        } else {
          await axios.patch(`https://todo.api.devcode.gethired.id/todo-items/${this.todoData.id}`, {
            title: this.todoData.title,
            priority: this.todoData.priority
          }).then(() => {
            this.$emit('close-modal')
            this.$emit('update-todos')
          })
        }
      } else {
        if (this.todoData.title === '' || this.todoData.priority === '') {
          console.log('field empty')
        } else {
          await axios.post(`https://todo.api.devcode.gethired.id/todo-items`, {
            activity_group_id: this.activityId,
            title: this.todoData.title,
            priority: this.todoData.priority,
            is_active: false
          }, {
            params: {
              email: 'mhmmdrezalif@gmail.com'
            }
          }).then(() => {
            this.$emit('close-modal')
            this.$emit('update-todos')
          })
        }
      }
    }
  },
  mounted() {
    if (this.mode === 'edit') {
      this.todoData = this.todo
    }
  },
}
</script>

<style scoped>

</style>

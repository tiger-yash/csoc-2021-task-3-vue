<template>
  <main>
    <aside class="mx-auto flex justify-between mt-24 px-4">
      <label for="add task" class="flex-1">
        <input
          type="text"
          name="add task"
          v-model="TaskData"
          class="
            todo-add-task-input
            px-4
            py-2
            placeholder-blueGray-300
            text-blueGray-600
            bg-white
            rounded
            text-sm
            border border-blueGray-300
            outline-none
            focus:outline-none
            focus:ring
            w-full
          "
          placeholder="Enter Task"
        />
      </label>
      <button
        type="button"
        class="
          todo-add-task
          bg-transparent
          hover:bg-green-500
          text-green-700 text-sm
          hover:text-white
          px-3
          py-2
          border border-green-500
          hover:border-transparent
          rounded
        "
        @click="addTask"
      >
        Add Task
      </button>
    </aside>
    <div class="mx-auto flex justify-center mt-10 px-4">
      <span
        class="
          inline-block
          justify-between
          bg-blue-600
          py-1
          mb-5
          px-9
          text-sm text-white
          font-bold
          rounded-full
        "
        >Available Tasks</span
      >
    </div>
  </main>
</template>

<script>
import {
  defineComponent,
  useContext,
  toRefs,
  reactive,
} from '@nuxtjs/composition-api'

export default defineComponent({
  emits: ['newTask'],
  setup(props, context) {
    const state = reactive({
      TaskData: '',
    })
    const { $axios, store, $toast } = useContext()
    function addTask() {
      /**
       * @todo Complete this function.
       * @todo 1. Send the request to add the task to the backend server.
       * @todo 2. Add the task in the dom.
       * @hint use emit to make a event that parent can observe
       */
      if (!state.TaskData) return
      const data = {
        title: state.TaskData,
      }

      const headers = {
        Authorization: 'Token ' + store.getters.token,
      }
      $axios
        .post('todo/create/', data, { headers })
        .then(function () {
          $toast.success('Task Added')

          state.TaskData = ''
          context.emit('newTask')
        })
        .catch((e) => {
          $toast.error(e)
        })
    }
    return {
      addTask,
      ...toRefs(state),
    }
  },
})
</script>

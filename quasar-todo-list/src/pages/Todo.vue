<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        class="col"
        square
        filled
        bg-color="white"
        v-model="newTask"
        label="Add task"
        dense
        v-on:keyup.enter="addTask"
      >
        <template v-slot:append>
          <q-btn round dense flat icon="add" v-on:click="addTask" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        v-ripple
        v-for="(task, index) in tasks"
        v-bind:key="task.title"
        v-on:click="task.done = !task.done"
        v-bind:class="{ 'done bg-blue-1': task.done }"
        clickable
      >
        <q-item-section avatar>
          <q-checkbox
            class="no-pointer-events"
            v-model="task.done"
            color="primary"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
            flat
            dense
            round
            color="primary"
            icon="delete"
            v-on:click.stop="deleteItem(index)"
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!tasks.length" class="no-task absolute-center">
      <q-icon name="check" size="100px" color="primary" />
      <div class="text-h5 text-primary text-center">
        No tasks
      </div>
    </div>
  </q-page>
</template>

<script>
export default {
  data: function() {
    return {
      newTask: "",
      tasks: [
        // {
        //   title: "Workout in the morning.",
        //   done: false
        // },
        // {
        //   title: "Code for 2 hours.",
        //   done: false
        // },
        // {
        //   title: "Meditate.",
        //   done: false
        // }
      ]
    };
  },
  methods: {
    deleteItem(index) {
      this.$q
        .dialog({
          dark: true,
          title: "Confirm",
          message: "Are you sure you would like to delete this task?",
          cancel: true,
          persistent: true
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.notify("Task has been deleted.");
        });
    },
    addTask() {
      if (this.newTask === '') {
        this.$q
          .dialog({
            title: "Whoops!",
            message: "Task input is empty. Try typing something first."
          })
          .onOk(() => {
          });
      } else {
        this.tasks.push({
          title: this.newTask,
          done: false
        });
        this.$q.notify("Task has been added!");
        this.newTask = "";
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}

.no-task {
  opacity: 0.5;
}
</style>

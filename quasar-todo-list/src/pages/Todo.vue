<template>
  <q-page class="bg-grey-3 column">
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
          <q-btn flat dense round color="primary" icon="delete" v-on:click.stop="deleteItem(index)" />
        </q-item-section>
      </q-item>
    </q-list>
  </q-page>
</template>

<script>
export default {
  data: function() {
    return {
      tasks: [
        {
          title: "Workout in the morning.",
          done: false
        },
        {
          title: "Code for 2 hours.",
          done: false
        },
        {
          title: "Meditate.",
          done: false
        }
      ]
    };
  },
  methods: {
    deleteItem(index) {
      this.$q.dialog({
        dark: true,
        title: 'Confirm',
        message: 'Are you sure you would like to delete this task?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tasks.splice(index, 1);
        this.$q.notify('Task has been deleted.')
      })
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
</style>

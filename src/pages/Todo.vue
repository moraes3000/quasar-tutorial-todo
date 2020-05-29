<template>
  <q-page class="bg-grey-3 column">
    <h5>Tarefa</h5>
    <div class="row q-pa-sm bg-primary">
      <q-input
        v-model="novaTarefa"
        @keyup.enter="addTask"
        class="col"
        squere
        filled
        bg-color="white"
        placeholder="add tarefa"
        dense
      >
        <template v-slot:append>
          <q-btn @click="addTask" round dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.done = !task.done"
        :class="{ 'done bg-blue-1' :task.done }"
        clickable
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox v-model="task.done" color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{task.title}}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn @click.stop="deleteTask(index)" flat round dense color="primary" icon="delete" />
        </q-item-section>
      </q-item>
    </q-list>
  </q-page>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  data() {
    return {
      novaTarefa: "",
      tasks: [
        {
          title: "Compra cerveja",
          done: false
        }
      ]
    };
  },
  methods: {
    deleteTask(index) {
      this.$q
        .dialog({
          title: "Confirm",
          message: "tem certeza que deseja remover esse item?",
          cancel: true,
          persistent: true
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.notify("item removido com sucesso");
        });
    },
    addTask() {
      // console.log("pego");
      this.tasks.push({
        title: this.novaTarefa,
        done: false
      });
      this.novaTarefa = "";
    }
  }
});
</script>

<style lang='scss'>
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbbbbb;
  }
}
</style>

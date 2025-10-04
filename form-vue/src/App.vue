<template>
  <div class="app">
    <h1>Gestor de tareas</h1>
    <div class="task-input">
      <input 
        v-model="newTask" 
        @keyup.enter="addTask"
        placeholder="Escribe el nombre de la tarea"
      />
      <button @click="addTask">Agregar</button>
    </div>
    <hr />
    <h2 class="titulo-listado">Listado de tareas</h2>
    <div v-if="tasks.length === 0" class="no-tasks">
      No hay tareas registradas.
    </div>
    <div v-else class="kanban">
      <div class="column">
        <h3>To do</h3>
        <div 
          v-for="(task, index) in tasks.filter(t => t.status === 'todo')" 
          :key="index"
          class="task todo"
        >
          <span>{{ task.name }}</span>
          <button class="arrow blue" @click="changeStatus(task, 'doing')">➡</button>
        </div>
      </div>
      <div class="column">
        <h3>Doing</h3>
        <div 
          v-for="(task, index) in tasks.filter(t => t.status === 'doing')" 
          :key="index"
          class="task doing"
        >
          <span>{{ task.name }}</span>
          <button class="arrow green" @click="changeStatus(task, 'done')">➡</button>
        </div>
      </div>
      <div class="column">
        <h3>Done</h3>
        <div 
          v-for="(task, index) in tasks.filter(t => t.status === 'done')" 
          :key="index"
          class="task done"
        >
          <span>{{ task.name }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: '',
      tasks: []
    }
  },
  methods: {
    addTask() {
      if (this.newTask.trim()) {
        this.tasks.push({
          name: this.newTask,
          status: 'todo'
        });
        this.newTask = '';
      }
    },
    changeStatus(task, nextStatus) {
      // Solo permite avanzar, nunca eliminar en Done
      if (task.status === 'todo' && nextStatus === 'doing') {
        task.status = 'doing';
      } else if (task.status === 'doing' && nextStatus === 'done') {
        task.status = 'done';
      }
    }
  }
}
</script>

<style>
.app { max-width: 700px; margin: auto; font-family: 'Segoe UI', sans-serif; }
.task-input { margin-bottom: 20px; display: flex; gap: 10px; }
input { flex: 1; padding: 6px; border-radius: 4px; border: 1px solid #bbb; }
button { padding: 6px 12px; border-radius: 4px; border: none; background: #eee; cursor: pointer; }
hr { margin: 20px 0; }
.titulo-listado { color: #2196f3; margin-bottom: 10px; }
.no-tasks { text-align: center; color: #f44336; font-size: 18px; margin: 30px 0; }
.kanban { display: flex; gap: 20px; }
.column { flex: 1; background: #fffbe7; padding: 10px; border-radius: 8px; border: 1px dashed #ccc; min-height: 120px; }
.column h3 { text-align: center; margin-bottom: 10px; }
.task { margin: 8px 0; padding: 8px; border-radius: 6px; display: flex; align-items: center; justify-content: space-between; font-size: 16px; }
.todo { border: 2px solid #2196f3; background: #e3f2fd; }
.doing { border: 2px solid #4caf50; background: #e8f5e9; }
.done { border: 2px solid #f44336; background: #ffebee; }
.arrow.blue { color: #2196f3; font-weight: bold; background: none; border: none; font-size: 20px; }
.arrow.green { color: #4caf50; font-weight: bold; background: none; border: none; font-size: 20px; }
.arrow:active { transform: scale(1.2); }
</style>
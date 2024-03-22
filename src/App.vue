<template>
  <div>
    <h1>Kanban Board</h1>
    <div class="canban">
        <KanbanContainer :listArr="TaskList"/>
    </div>
  </div>
</template>

<script>
import KanbanContainer from "./components/KanbanContainer.vue"

export default {
  name: 'App',
  data() {
    return {
      TaskList: [
        {
          id: 1,
          name: "Название1",
          listId: 1
        },
        {
          id: 2,
          name: "Название2",
          listId: 1
        },
        {
          id: 3,
          name: "Название3",
          listId: 2
        },
        {
          id: 4,
          name: "Название4",
          listId: 2
        },
        {
          id: 5,
          name: "Название5",
          listId: 1
        },
        {
          id: 6,
          name: "Название6",
          listId: 2
        }
      ]
    }
  },
  components: {
    KanbanContainer
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 20px;
}
.canban{
  display: flex;
  justify-content: center;
}
</style>

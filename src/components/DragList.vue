<!-- <template>
    <div @drop="onDrop" @dragover.prevent class="cards">
        <div v-for="(task, index) in tasks" :key="task.id" class="task_card"
            draggable="true" @dragstart="onDragStart(index)">
            {{ task.name }}
        </div>
    </div>
</template>

<script>
export default {
    name: "DragList",
    data() {
        return {
            dragIndex: null,
        }
    },
    computed: {
    },
    methods: {
        onDragStart(index) {
            this.dragIndex = index;
        },
        onDrop(event) {
            event.preventDefault();
            const targetIndex = this.tasks.length;
            
            // Создаем копию массива задач
            const tasksCopy = this.tasks.slice();
            
            // Получаем данные о перетаскиваемом элементе
            const draggedTask = this.tasks[this.dragIndex];
            
            // Вставляем перетаскиваемый элемент в новое место
            tasksCopy.splice(targetIndex, 0, draggedTask);
            
            // Обновляем локальное состояние задач внутри компонента
            this.tasks = tasksCopy;
        }
    },
    props: {
        tasks: Array
    }
}
</script>

<style scoped>
.task_card {
    min-height: 20px;
    border-radius: 20px;
    border: 1px solid black;
    width: 80%;
    margin: 5%;
}
.cards{
    min-height: 200px;
}
</style> -->
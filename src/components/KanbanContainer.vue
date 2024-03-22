<template>
    <div class="canban-cont">
        <!--Здесь функционал для добавления в контейнеры данных-->
        <div class="new-items">
            <div class="new-cont">
                <button @click="AddNewItem()">
                    Добавить
                </button>
            </div>
            <div class="new-cont">
                <input type="text" v-model="newItem" placeholder="Введите название">
            </div>
        </div>
        <div class="list-cont">
            <div v-for="(list, indList) in task_list" :key="list.id" class="list" @drop="onDrop(indList, index)"
                @dragover.prevent @dragenter.prevent>
                Лист {{ list.id }}
                <!-- <p>Кол-во элементов: {{ list.tasks.length }}</p> -->
                <div>
                    <div class="cards">
                        <div v-for="(task, index) in list.tasks" :key="task.id" class="task_card" draggable="true" @dragstart="onDragStart(index, indList)" @dragover.prevent @dragenter.prevent @drop="onDrop(indList, index)">
                            {{ task.name }}
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "KanbanBoard",
    data() {
        return {
            newItem: "",
            task_list: [],
            draggedTask: null
        }
    },
    methods: {
        //перед добавлением на страницу их нужно отсортировать по листам
        SortItems: async function() {
            let lists = []
            const len = this.task_list.length;
            for(let i = 0; i < len; i++) {
                let list = {
                    id: this.task_list[i].listId,
                    tasks: []
                }
                await this.AddnewList(lists, list)
            }
            await this.AddAllTasks(lists, this.task_list);
            //this.task_list = sortElem;
            //console.log(this.task_list);
        },
        //добавляем для начала листы
        AddnewList: async function(lists, NewList) {
            const len = lists.length;
            let created = false;
                if(lists.length > 0) {
                    for(let i = 0; i < len; i++) {
                    if(lists[i].id == NewList.id) {
                        created = true;
                        break;
                    }
                }
            }
            if(created == false) {
                lists.push(NewList);
            }
        },
        //добавляем в эти листы задачи
        AddAllTasks: async function(lists, tasks) {
            const len = tasks.length;
            for(let i = 0; i < len; i++) {
                for(let j = 0; j < lists.length; j++) {
                    if(tasks[i].listId == lists[j].id) {
                        lists[j].tasks.push(tasks[i])
                        break;
                    }
                }
            }
            this.task_list = lists;
        },
        //добавление нового элемента
        AddNewItem: async function() {
            let item = {
                name: this.newItem,
                listId: 1,
                id: Math.floor(Math.random() * 10) + 12
            }
            this.task_list[0].tasks.push(item);
            this.newItem = "";
        },
        //начало перетаскивания задачи
        onDragStart(taskIndex, listIndex) {
            this.draggedTask = { taskIndex, listIndex };

        },
        // Метод вызывается при завершении перетаскивания задачи
        onDrop(targetListIndex, targetTaskIndex) {
            // Проверяем, был ли начат процесс перетаскивания
            if (this.draggedTask) {
                let { taskIndex, listIndex } = this.draggedTask;

                // Извлекаем задачу из исходного списка
                let draggedTask = this.task_list[listIndex].tasks.splice(taskIndex, 1)[0];

                // Вставляем перемещенную задачу в целевой список по указанным индексам
                if (listIndex === targetListIndex && taskIndex < targetTaskIndex) {
                    targetTaskIndex--;
                }
                this.task_list[targetListIndex].tasks.splice(targetTaskIndex, 0, draggedTask);

                // Сбрасываем информацию о перетаскиваемой задаче
                this.draggedTask = null;
            }
        }
    },
    props: {
        listArr: Array
    },
    async mounted() {
        this.task_list = this.listArr;
        await this.SortItems()
    },
}
</script>

<style scoped>
.canban-cont{
    margin: 1%;
    padding: 1%;
    min-width: 90%;
    border-radius: 20px;
    min-height: 500px;
    background-color: grey;
}
.new-items{
    display: flex;
    justify-content: left;
}
.new-cont{
    margin: 1%;
}
.new-cont input {
    padding: 5px;
}
.new-cont button {
    padding: 7px;
    border: none;
    outline: none;
}
.list-cont{
    display: flex;
}
.list{
    padding-top: 20px;
    margin: 1%;
    background-color: white;
    padding: 5px;
    border-radius: 20px;
    min-width: 200px;
    min-height: 250px;
}
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
</style>
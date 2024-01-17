<script setup>
import { ref } from 'vue';

const tasks = ref([
    { name: 'Task 1', time: 30 },
    { name: 'Task 2', time: 40 },
    { name: 'Task 3', time: 60 },
    { name: 'Task 4', time: 45 },
    { name: 'Task 5', time: 50 },
]);

const isEditFormVisible = ref(false);
const editedTask = ref({ name: '', time: 0 });
const editedTaskIndex = ref(null);

const openEditForm = (index) => {
    editedTask.value = { ...tasks.value[index] };
    editedTaskIndex.value = index;
    isEditFormVisible.value = true;
};

const updateTask = () => {
    if (editedTaskIndex.value !== null) {
        tasks.value[editedTaskIndex.value] = { ...editedTask.value };
        isEditFormVisible.value = false;
        editedTask.value = { name: '', time: 0 };
        editedTaskIndex.value = null;
    }
};
</script>

<template>
    
    <div class="container mx-auto my-8 bg-green-100 p-8 rounded shadow-md">
        <div v-for="(task, index) in tasks" :key="index" class="mb-4 bg-green-300 p-4 rounded shadow-md">
            <div class="flex justify-between items-center">
                <div>
                    <span class="text-lg font-semibold">{{ task.name }}</span>
                    <span class="ml-2 text-gray-500">{{ task.time }} minutes</span>
                </div>
                <button @click="openEditForm(index)" class="ml-7 bg-green-500 text-white px-6 py-3 rounded hover:bg-green-600">
                    Edit
                </button>
            </div>
        </div>

        <transition name="fade">
            <div v-if="isEditFormVisible"
                class="fixed top-0 left-0 w-full h-full flex items-center justify-center bg-black ">
                <div class="bg-green-100 p-8 rounded shadow-md">
                    <h2 class="text-2xl font-semibold mb-6 text-green-600">Edit Task</h2>
                    <form @submit.prevent="updateTask">
                        <div class="mb-6">
                            <label for="editName" class="block text-sm font-medium text-green-500">Name</label>
                            <input v-model="editedTask.name" type="text" id="editName" name="editName"
                                class="mt-1 p-3 border border-gray-300 rounded w-full focus:outline-none focus:border-blue-500"
                                required>
                        </div>
                        <div class="mb-6">
                            <label for="editTime" class="block text-sm font-medium text-green-500">Time (minutes)</label>
                            <input v-model="editedTask.time" type="number" id="editTime" name="editTime"
                                class="mt-1 p-3 border border-gray-300 rounded w-full focus:outline-none focus:border-blue-500"
                                required>
                        </div>
                        <div class="flex justify-end">
                            <button type="submit" class="bg-green-500 text-white px-6 py-3 rounded hover:bg-green-600">
                                Submit
                            </button>  
                        </div>
                    </form>
                </div>
            </div>
        </transition>
    </div>
</template>


<style>
.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.5s;
}

.fade-enter,
.fade-leave-to {
    opacity: 0;
}</style>

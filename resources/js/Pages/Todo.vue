<template>
    <Head title="Dashboard" />

    <BreezeAuthenticatedLayout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                Todo
            </h2>
        </template>

         <!-- component -->
        <div class="h-100 w-full flex items-center justify-center bg-teal-lightest font-sans">
            <div class="bg-white rounded shadow p-6 m-4 w-full lg:w-3/4 lg:max-w-lg">
                <div class="mb-4">
                    <h1 class="text-grey-darkest">Todo List</h1>
                    <div class="flex mt-4">

                        <input class="shadow appearance-none border rounded w-full py-2 px-3 mr-4 text-grey-darker" placeholder="Add Todo" v-model="form.content">

                        <button class="flex-no-shrink p-2 border-2 rounded-lg text-teal border-teal text-white bg-blue-500 hover:bg-blue-700" @click="submit">Add</button>
                    </div>
                </div>
                <div>
                    <div class="flex mb-4 items-center" v-for="todo in todos" :key="todo.id">

                        <p class="w-full text-grey-darkest">{{ todo.content }}</p>

                        <button v-if="todo.is_done" class="flex-no-shrink w-1/3 p-2 ml-4 mr-2 border-2 rounded-lg border-grey" @click="updateStatus(todo)">Not Done</button>

                        <button v-else class="flex-no-shrink p-2 ml-4 mr-2 border-2 rounded-lg border-green text-white bg-green-500 hover:bg-green-700" @click="updateStatus(todo)">Done</button>
              

                        <button class="flex-no-shrink p-2 ml-2 border-2 rounded-lg text-red border-red text-white bg-red-500 hover:bg-red-700"  @click="deleteTodo(todo)">Remove</button>
                    </div>
                </div>
            </div>
        </div>

    </BreezeAuthenticatedLayout>
</template>

<script>
import BreezeAuthenticatedLayout from '@/Layouts/Authenticated.vue'
import { Head } from '@inertiajs/inertia-vue3';
import { Inertia } from '@inertiajs/inertia'
import { reactive } from 'vue';

export default {
    components: {
        BreezeAuthenticatedLayout,
        Head,
    },
    props:{
        todos: Object
    },
    setup(){
        const form = reactive({
            content: null
        });

        function submit(){
            if(form.content != null){
                Inertia.post('/todos',form);
                form.content = null;
            }
        }

        function updateStatus(todo){
            Inertia.put('/todos/'+todo.id+'/update');
        }

        function deleteTodo(todo){
            Inertia.delete('/todos/'+todo.id);
        }

        

        return {
            form,
            submit,
            updateStatus,
            deleteTodo
        }
    }
}
</script>

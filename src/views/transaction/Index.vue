<template>
    <div class="container">
        <div class="row">
            <div class="col-md">
                <h2 class="text-center">User List</h2>
                <div class="row">
                    <div class="col-md-3 offset-9">
                        <router-link class="btn btn-primary w-100" :to="{ name: 'transaction.create' }">Add Data</router-link>
                    </div>
                </div>
                    <keep-alive>
                        <table class="table">
                            <thead>
                                <tr>
                                    <th scope="col">No.</th>
                                    <th scope="col">Email</th>
                                    <th scope="col">Full Name</th>
                                    <th scope="col">Photo</th>
                                    <th scope="col">Action</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(list, index) in userList.data" :key="index">
                                    <th scope="row">{{ list.id }}</th>
                                    <td>{{ list.email }}</td>
                                    <td>{{list.first_name}} {{list.last_name}}</td>
                                    <td><img :src="list.avatar"></td>
                                    <td>
                                        <router-link class="btn btn-primary d-block w-100 my-1" 
                                        :to="{name: 'transaction.edit', 
                                        params:{id: list.id}}"
                                        >Edit</router-link>
                                        <button class="btn btn-danger d-block w-100 my-1"
                                        @click.prevent="destroy(list.id, index)"
                                        >Delete</button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </keep-alive>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import {onMounted, ref} from 'vue';

export default {
    setup(){
        let userList = ref([])

        onMounted(() => {
            axios.get('https://reqres.in/api/users')
            .then((result) => {
                userList.value = result.data
            }).catch((err) => {
                console.log(err.response)
            })
        })

        function destroy(id, index){
            axios.delete(`https://reqres.in/api/users/${id}`)
            .then((result) => {
                userList.value.data.splice(index, 1)
            }).catch((err) => {
                validation.value = err.response.data
            })
        }

        return{
            userList,
            destroy
        }
    }
}
</script>
<template>
    <div class="container">
        <div class="row">
            <div class="col-md text-center">
                <h2>Add Data</h2>
            </div>
        </div>
        <div class="row">
            <div class="col-md">
                <form @submit.prevent="storeData()">
                    <div class="mb-3">
                        <label class="form-label">Email address</label>
                        <input type="email" class="form-control" v-model="dataUser.email">
                        <div class="text-danger" v-if="validation.email">
                            {{validation.email[0]}}
                        </div>
                    </div>
                    <div class="mb-3">
                        <label class="form-label">First Name</label>
                        <input type="text" class="form-control" v-model="dataUser.first_name">
                        <div class="text-danger" v-if="validation.first_name">
                            {{validation.first_name[0]}}
                        </div>
                    </div>
                    <div class="mb-3">
                        <label class="form-label">Last Name</label>
                        <input type="text" class="form-control" v-model="dataUser.last_name">
                        <div class="text-danger" v-if="validation.last_name">
                            {{validation.last_name[0]}}
                        </div>
                    </div>
                    <div class="mb-3">
                        <label class="form-label">Photo</label>
                        <input type="text" class="form-control" v-model="dataUser.photo">
                        <div class="text-danger" v-if="validation.photo">
                            {{validation.photo[0]}}
                        </div>
                    </div>
                    <button type="submit" class="btn btn-primary">Submit</button>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
import { reactive, ref } from 'vue';
import { useRouter } from 'vue-router';
import axios from 'axios';

export default {
    setup(){
        const dataUser = reactive({
            email: '',
            first_name: '',
            last_name: '',
            photo: ''
        })

        const validation = ref([])

        const router = useRouter()

        function storeData(){
            axios.post('https://reqres.in/api/users', dataUser)
            .then((result) => {
                router.push({
                    name: 'transaction.index'
                })
            }).catch((err) => {
                validation.value = err.response.data
            })
        }

        return {
            dataUser,
            validation,
            router,
            storeData
        }
    }
}
</script>
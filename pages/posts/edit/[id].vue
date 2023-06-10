<script setup lang="ts">

    //meta title
    useHead({
        title: 'Edit a Post - SantriKoding.com',
    });

    //init config
    const config = useRuntimeConfig();  

    //init router
    const router = useRouter();

    //init route
    const route = useRoute();

    //fetch daa for get detail data post
    const { data: post } : any = await useFetch(`${config.public.apiBase}/api/posts/${route.params.id}`);
    

    //define state
    const image     = ref('');
    const title     = ref(post.value.data.title);
    const content   = ref(post.value.data.content);
    const errors  : any  = ref({});

    //method for handle file changes
    const handleFileChange = ( e: any ) => {

        //assign file to state 
        image.value = e.target.files[0]
    }

    //method "updatePost"
    const updatePost = async () => {
        
        //init formData
        let formData = new FormData();

        //assign state value to formData
        formData.append('image', image.value);
        formData.append('title', title.value);
        formData.append('content', content.value);
        formData.append('_method', 'PATCH');

        //store data with API
        await $fetch(`${config.public.apiBase}/api/posts/${route.params.id}`, {

            //method
            method: 'POST',

            //data
            body: formData
        })
        .then(() => {
            //redirect
            router.push({ path: "/posts" });
        })
        .catch((error) => {

            //assign response error data to state "errors"
            errors.value = error.data
        });
    }

</script>

<template>
    <div class="container mt-5">
        <div class="row">
            <div class="col-md-12">
                <div class="card border-0 rounded shadow">
                    <div class="card-body">
                        <form @submit.prevent="updatePost()">
                            <div class="mb-3">
                                <label class="form-label fw-bold">Image</label>
                                <input type="file" class="form-control" @change="handleFileChange($event)">
                                <div v-if="errors.image" class="alert alert-danger mt-2">
                                    <span>{{ errors.image[0] }}</span>
                                </div>
                            </div>
                            <div class="mb-3">
                                <label class="form-label fw-bold">Title</label>
                                <input type="text" class="form-control" v-model="title" placeholder="Title Post">
                                <div v-if="errors.title" class="alert alert-danger mt-2">
                                    <span>{{ errors.title[0] }}</span>
                                </div>
                            </div>
                            <div class="mb-3">
                                <label class="form-label fw-bold">Content</label>
                                <textarea class="form-control" v-model="content" rows="5" placeholder="Content Post"></textarea>
                                <div v-if="errors.content" class="alert alert-danger mt-2">
                                    <span>{{ errors.content[0] }}</span>
                                </div>
                            </div>
                            <button type="submit" class="btn btn-md btn-primary rounded-sm shadow border-0">Update</button>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
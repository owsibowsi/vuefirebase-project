<template>
    <div class="create">
        <h1>Lets share some hot stuff</h1>
        <form class="createform">
            <input type="text" v-model="post.title" placeholder="Title here bro" required>
            <input type="text" v-model="post.description" placeholder="Type a description here" required>
            <input type="file" ref="fileInput" accept="image/*" v-on:change="previewImage">
            <div>
                <img :src="post.img" class="image-preview">
            </div>
            <button type="button" v-on:click="createPost">Create post</button>
        </form>
    </div>
</template>

<script>
import { postRef } from '../firebase-db'
export default {
    data () {
        return {
            post: {
                description: '',
                img: null,
                title: '',
            }
        }
    },
    methods: {
        previewImage () {
            const imageFile = this.$refs.fileInput.files[0]
            const fileReader = new FileReader()
            fileReader.onload = (event) => {
                this.post.img = event.target.result
            }
            fileReader.readAsDataURL(imageFile)
        },
        createPost () {
            postRef.add(this.post)
            this.$router.push('/')
        }
    }
    
}
</script>

<style>

html{
    color: #34495e;
    display: flex;
    justify-content: center;
}

.create{
    background-color: #41b883;
    max-width: 700px;
    border-radius: 5px;
    padding: 20px;
    
}

.createform{
    display: flex;
    flex-direction: column;
    max-width: 200px;
    justify-content: center;
    align-items: center;
    
}

.create{
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
}

input{
    margin: 5px 0;
}

h1{
    color: #34495e;
}
</style>
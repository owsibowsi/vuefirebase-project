<template>

    <section class="SPA">
    <div class="grid-container">
        <article v-for="post in posts" :key="post.id" class="posts">
            <h2> {{post.title}} </h2>
            <img :src="post.img" alt="" class="articleimg">
            <h3> {{post.description}} </h3>
            <button class="updatebutton" type="button" v-on:click="selectPost(post.id, post.title, post.img, post.description)">Edit post</button>
            <button class="deletebutton" type="button" v-on:click="DeletePost(post.id)">Delete post</button>
        </article>
    </div>

<div>
    <form class="form-container">
        <input v-model="postUpdate.title" value:postUpdate.title class="uform">
        <input v-model="postUpdate.description" value:postUpdate.description class="uform">
        <input type="file" ref="fileInput" accept="image/*" change="previewImage" class="uform">

        <div>
            <img :src="postUpdate.img" class="image-preview">
        </div>

        <button type="button" class="updatebutton" v-on:click="UpdatePost()"> Update</button>
    </form>
</div>
</section>
</template>

<script>
import { postRef } from '../firebase-db'
export default {
    data () {
        return {
            posts: [],
            postUpdate:{
                id: "",
                title: "",
                img: "",
                description: ""
            },
        }
    },
    firestore: {
        posts: postRef
    },
    methods: {
        selectPost(postId, titleId, imgId, descriptionId){
            this.postUpdate.id = postId
            this.postUpdate.title = titleId
            this.postUpdate.img = imgId
            this.postUpdate.description = descriptionId
        },
        UpdatePost(){
            postRef.doc(this.postUpdate.id).update(this.postUpdate)
            this.postUpdate = {
                id: "",
                title: "",
                img: "",
                description: ""
            }
        },

        CloseUpdatePost(){

        },
        DeletePost(id){
            postRef.doc(id).delete()
        },
        previewImage() {
            const imagefile = this.$refs.fileInput.files[0]
            const fileReader = new fileReader()
            fileReader.onload = (event) => {
                this.postUpdate.img = event.target.result
            }
            fileReader.readAsDataURL(imagefile)
        }
        
    }
}
</script>

<style>
.grid-container{
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-gap: 4%;
}

.grid-container img{
    max-height: 250px;
}

.form-container{
    margin: 130px 0;
}

.posts{
    background-color: #41b883;
    color: #385162;
    border-radius: 5px;
}

.articleimg{
    border: 10px solid #385162;
    border-radius: 5px;
}

.deletebutton{
    color: white;
    background-color: red;
    border-color: red;
}

.updatebutton{
    background-color: #385162;
    border-color: #385162;
    color: white;
}

html{
    background-color: #385162;
}

.form-container{
    background-color: #41b883;
    width: 40%;
    padding: 20px;
    border-radius: 5px;
    margin: 100px auto;
}


</style>
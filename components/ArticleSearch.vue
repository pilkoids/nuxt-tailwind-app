<template>
    <div>
        <label class="block uppercase tracking-wide text-grey-darker text-xs font-bold mb-2" for="grid-first-name">
            Search by keyword: 
        </label><div></div>
      <input class="appearance-none block w-full bg-grey-lighter text-grey-darker border border-red rounded py-3 px-4 mb-3" id="grid-first-name" type="text" placeholder="Keyword">
        <p>{{ name }}</p>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    data(){
        return{
            name: this.getArticles()
        }
    },
    mounted(){
        
    },
    methods:{
        getArticles(){
            axios.get('https://jsonplaceholder.typicode.com/todos/1')
            .then((res) => {
                console.log("result::::::::::::::::::::", res.data.title);
                this.name =  res.data.title
            })
        }
    }
}
</script>

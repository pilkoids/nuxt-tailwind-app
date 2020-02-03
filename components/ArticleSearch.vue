<template>
    <div>
        <label class="block uppercase tracking-wide text-grey-darker text-xs font-bold mb-2" for="grid-first-name">
            Search news articles by keyword: 
        </label>
        
        <input v-model="qry" class="appearance-none block w-full bg-grey-lighter text-grey-darker border border-red rounded py-3 px-4 mb-3" id="grid-first-name" type="text" placeholder="Keyword">
        
        <button v-on:click="search()" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
            Search
        </button>
        
        <ul class="articles">
            <li v-for="n in name" v-bind:key="n" style="margin-bottom:10px;padding:10px;background:lightgrey">
                <p>{{n.url}}</p>
                <p>{{n.description}}</p>
                <p>Polarity: {{n.polarity}}</p>
            </li>
        </ul>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    props:['value'],
    data(){
        return{
            name:[],
            qry:""
        }
    },
    mounted(){
        
    },
    methods:{
        search(){
            console.log("Search clicked");
            console.log("Value:", this.qry)
            this.getArticles(this.qry);
        },
        getArticles(){
        axios({
            "method":"GET",
            "url":"https://microsoft-azure-bing-news-search-v1.p.rapidapi.com/search",
            "headers":{
            "content-type":"application/octet-stream",
            "x-rapidapi-host":"microsoft-azure-bing-news-search-v1.p.rapidapi.com",
            "x-rapidapi-key":"QIiovCTHWrmshe86Hfn2WFwYBPrZp1MqwVTjsnBYXNuAy0zGFS"
            },"params":{
                "count":"1",
                "q":this.qry
            }
            })
            .then((response)=>{
                this.name = response.data.value;
                for(var article in this.name){
                    console.log("article url:",this.name[article].url);

                    axios({
                        "method":"GET",
                        "url":"https://aylien-text.p.rapidapi.com/sentiment",
                        "headers":{
                        "content-type":"application/octet-stream",
                        "x-rapidapi-host":"aylien-text.p.rapidapi.com",
                        "x-rapidapi-key":"QIiovCTHWrmshe86Hfn2WFwYBPrZp1MqwVTjsnBYXNuAy0zGFS"
                        },"params":{
                        "url":this.name[article].url,
                        "mode":"tweet"
                        }
                        })
                        .then((response)=>{
                            console.log("Aylien response:", response.data.polarity);
                            this.name[article].polarity = response.data.polarity;

                            console.log("result---------------", response);
                            console.log("polarity:::", this.name[article].polarity)
                            console.log("this.name", this.name)
                        })
                        .catch((error)=>{
                            console.log(error)
                        })

                    this.name[article].test123 = "asdfasdfads";
                }
            })
            .catch((error)=>{
                console.log(error)
            })
        }
    }
}
</script>
<style scoped>
.articles{
    margin-top:30px;
}
</style>
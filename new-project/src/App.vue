<template>
      <div>
        <h1> Web Scraper</h1>
        <div class="wrapper">
            <div v-for="(article,index) in lastestArticles" :key="index">
                <span v-text="article.title"></span>
                <hr>
            </div>
        </div>
      </div>
</template>

<script>
import axios from 'axios';
import cheerio from 'cheerio';

export default {
  data() {
    return {
      lastestArticles: [], // defined empty array variable 
    }
  },

  methods:{
      getWebsiteData(){
      let self = this;
      let url = 'https://www.msn.com/en-au?AR=1'; // url we get data from
      let dataArray = [];                         // we put data in this array
      // GET request for remote image in node.js
      axios({
        method: 'get',
        url: url,
      })
      .then(function (response) {
          let html = response.data;

          let $ = cheerio.load(html);

          $("ul.tertiary li").each(function(){
                const title = $(this).find('h3').attr('aria-label');
                const image  = $(this).find('img').attr('src');

                // putting data in array.

                dataArray.push({
                  'title': title,
                  'image': image
                })
          });

          self.lastestArticles = dataArray;     // Here we assign value to vuejs variable
          
          console.log(dataArray);
      });
    }
    },
    created(){
      
    }

};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

<script setup>
import { ref } from 'vue';

import ArticleForm from './components/ArticleForm.vue';
import ArticleList from './components/ArticleList.vue';

import { ARTICLES_API } from './const/config.js';

const articles = ref([]);
getArticlesFromApi();

function addArticle(article){
    console.log('App.addArticle', article);

    articles.value.push(article);
    postArticleToApi(article);
    console.log('App.articles', articles.value);
}

function getArticlesFromApi(){
  fetch(ARTICLES_API).then(response => {
      response.json().then(json => {
          json.forEach(a => {
            const article = { id: a.id, title: a.title, description: a.description};
            articles.value.push(article);
        });
      });
    });
}

function postArticleToApi(article){    
  fetch(ARTICLES_API,  {
      method: "POST",
      headers: {
      "Content-Type": "application/json",
      },
      body: JSON.stringify(article)
  }).then();    
}

</script>

<template>
  <header>
    <h1 class="text-center">Vue JS Blog</h1>
  </header>

  <main>
    <section class="col col-4 p-1 v-align-top">
      <h2 class="text-center">Add article</h2>

      <ArticleForm @add-article="addArticle"/>
    </section>

    <section class="col col-8 v-align-top">
      <h2 class="text-center">Article list</h2>

      <ArticleList :articles="articles"/>
    </section>
  </main>
</template>

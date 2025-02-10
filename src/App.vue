<script setup>
import { ref } from 'vue';

import ArticleForm from './components/ArticleForm.vue';
import ArticleList from './components/ArticleList.vue';

const articles = ref([]);
getArticlesFromApi();

function addArticle(article){
    console.log('App.addArticle', article);

    articles.value.push(article);
    console.log('App.articles', articles.value);
}

function getArticlesFromApi(){
    const ARTICLES_API = 'https://6797ded3c2c861de0c6e4858.mockapi.io/articles';
    fetch(ARTICLES_API).then(response => {
      response.json().then(json => {
          json.forEach(a => {
            const article = { id: a.id, title: a.title, description: a.description};
            articles.value.push(article);
        });
      });
    });
}

</script>

<template>
  <header>
    <h1 class="text-center">Vue JS Blog</h1>
  </header>

  <main>
    <section class="col col-4">
      <h2 class="text-center">Add article</h2>

      <ArticleForm @add-article="addArticle"/>
    </section>

    <section class="col col-8">
      <h2 class="text-center">Article list</h2>

      <ArticleList :articles="articles"/>
    </section>
  </main>
</template>

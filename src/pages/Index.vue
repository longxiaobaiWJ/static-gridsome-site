<template>
  <Layout>
    <!-- Page Header -->
    <header class="masthead" style="background-image: url('/img/home-bg.jpg')">
      <div class="overlay"></div>
      <div class="container">
        <div class="row">
          <div class="col-lg-8 col-md-10 mx-auto">
            <div class="site-heading">
              <h1>Clean Blog</h1>
              <span class="subheading">A Blog Theme by Start Bootstrap</span>
            </div>
          </div>
        </div>
      </div>
    </header>

    <!-- Main Content -->
    <div class="container">
      <div class="row">
        <div class="col-lg-8 col-md-10 mx-auto">
          <div v-for="edge in $page.articles.edges" :key="edge.id">
            <div class="post-preview">
              <g-link :to="edge.node.path">
                <h2 class="post-title">{{ edge.node.title }}</h2>
                <h3 class="post-subtitle">{{ edge.node.description }}</h3>
              </g-link>
              <p class="post-meta">
                Posted by
                <a href="#">{{ edge.node.created_by.firstname }} {{ edge.node.created_by.lastname }}</a>
                on {{ edge.node.updated_at }}
              </p>
              <p>
                <span v-for="tag in edge.node.tags" :key="tag.id">
                  <a href="javascript:void(0);">{{ tag.title }}</a>&nbsp;&nbsp;
                </span>
              </p>
            </div>
            <hr />
          </div>
          <!-- Pager -->
          <div class="clearfix">
            <Pager :info="$page.articles.pageInfo"/>
          </div>
        </div>
      </div>
    </div>
  </Layout>
</template>

<page-query>
query ($page: Int) {
  articles: allStrapiArticle (perPage: 2, page: $page) @paginate {
    pageInfo {
      totalPages
      currentPage
    },
    edges {
      node {
        id,
        title,
        description,
        updated_at,
        created_by {
          firstname
          lastname
        },
        tags {
          id,
          title
        },
        path
      }
    }
  }
}
</page-query>

<script>
import { Pager } from 'gridsome'

export default {
  name: 'HomePage',
  metaInfo: {
    title: "Hello, world!"
  },
  components: {
    Pager
  }
}
</script>

<style>
.home-links a {
  margin-right: 1rem;
}
.clearfix a {
  margin: 0 0.5rem;
  padding: 0 0.5rem;
}
</style>

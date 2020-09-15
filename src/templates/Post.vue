<template>
	<Layout>
		<!-- Page Header -->
		<header 
			class="masthead" 
			:style="{
				backgroundImage:  SiteCompleted($page.strapiArticle.cover.url) 
			}">
			<div class="overlay"></div>
			<div class="container">
				<div class="row">
					<div class="col-lg-8 col-md-10 mx-auto">
						<div class="post-heading">
							<h1>{{ $page.strapiArticle.title }}</h1>
							<h2 class="subheading">{{ $page.strapiArticle.description }}</h2>
							<span class="meta">Posted by
								<a href="#">{{ $page.strapiArticle.created_by.firstname }} {{ $page.strapiArticle.created_by.lastname }}</a>
								on {{ $page.strapiArticle.updated_at }}</span>
						</div>
					</div>
				</div>
			</div>
		</header>

		<!-- Post Content -->
		<article>
			<div class="container">
				<div class="row">
					<div class="col-lg-8 col-md-10 mx-auto" v-html="MarkdownItToHtml($page.strapiArticle.content)">
					</div>
				</div>
			</div>
		</article>
	</Layout>
</template>

<page-query>
query ($id: ID!) {
	strapiArticle (id: $id) {
    id,
    title,
    description,
    content,
    cover {
      url
    },
		updated_at,
    created_by {
      firstname,
      lastname
    }
  }
}
</page-query>

<script>
import MarkdownIt from 'markdown-it'

export default {
	name: 'PostPage',
	methods: {
		MarkdownItToHtml (content) {
			return new MarkdownIt().render(content)
		},
		SiteCompleted (suffix) {
			return `url(${process.env.GRIDSOME_API_URL}${suffix})`
		}
	}
}
</script>

<style>

</style>
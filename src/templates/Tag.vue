<template>
  <Layout>
    <div class="container-inner mx-auto my-16">
      <h2 class="text-4xl font-bold mb-8 border-b">Tag: {{ $page.tag.title }}</h2>
      <div class="flex flex-wrap -mx-2">
        
      <div v-for="post in $page.tag.belongsTo.edges" :key="post.node.id" class="post w-1/1 sm:w-1/3 border-gray-400 border-b mb-12 overflow-x-auto">
        <div class="max-w-sm overflow-hidden shadow-lg mx-6">
          <g-image class="w-full rounded" :src="post.node.image" alt="Sunset in the mountains"/>
          <div class="px-6 py-4">
        <h2 class="text-3xl font-bold"><g-link :to="post.node.path" class="text-copy-primary">{{ post.node.title }}</g-link></h2>
        <div class="text-copy-secondary mb-4 hidden">
          <span>{{ post.node.date }}</span>
          <span> &middot; </span>
          <span>{{ post.node.timeToRead }} min read</span>
        </div>

        <div class="hidden text-lg mb-4">
          {{ post.node.summary }}
        </div>
        <div class="mb-8 hidden">
          <g-link :to="post.node.path" class="font-bold uppercase">Read More</g-link>
        </div>
        <div class="px-6 py-4 hidden">
          <span class="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2">{{ $page.tag.title }}</span>
          <span class="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2">#travel</span>
          <span class="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700">#winter</span>
        </div>
      </div>
          </div>
        </div>
      </div>

      <pagination-posts
        v-if="$page.tag.belongsTo.pageInfo.totalPages > 1"
        :base="`/tag/${$page.tag.title}`"
        :totalPages="$page.tag.belongsTo.pageInfo.totalPages"
        :currentPage="$page.tag.belongsTo.pageInfo.currentPage"
      />

    </div>
  </Layout>
</template>

<page-query>
query Tag ($id: ID!, $page: Int) {
  tag: tag (id: $id) {
    title
    belongsTo (page: $page, perPage: 9) @paginate {
      totalCount
      pageInfo {
        totalPages
        currentPage
      }
      edges {
        node {
          ...on Post {
            title
            timeToRead
    	      date (format: "MMMM D, YYYY")
            path
            summary
            tags {
              title
              path
            }
            image
          }
        }
      }
    }
  }
}
</page-query>

<script>
import PaginationPosts from '../components/PaginationPosts'

export default {
  metaInfo() {
    return {
      title: 'Tag: ' + this.$page.tag.title
    }
  },
  components: {
    PaginationPosts
  }
}
</script>

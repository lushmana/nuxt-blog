<template>
  <div class="page-index">
    <div class="pageHeader">Andrew Lush's Blog</div>
    <hr/>
    <div class="container">
      <BlogSection sectionTitle="Blogs" :blogs="blogs"/>
      <!-- <hr/>
      <BlogSection sectionTitle="Portfolio" :blogs="portfolio"/> -->
    </div>
  </div>
</template>

<script>
  import BlogSection from "~/components/Sections/BlogSection"

  import blogsEn from '~/contents/en/blogsEn.js'
//  import blogsEs from '~/contents/es/blogsEs.js'
//  import portfolioEn from '~/contents/en/portfolioEn.js'

  export default {
    async asyncData ({app}) {

      const blogs = app.i18n.locale === 'en' ? blogsEn : blogsEs
//      const portfolio = portfolioEn
      
      async function asyncImport (blogName) {
        const wholeMD = await import(`~/contents/${app.i18n.locale}/blog/${blogName}.md`)
        return wholeMD.attributes
      }

      return Promise.all(blogs.map(blog => asyncImport(blog)))
      .then((res) => {
        return {
          blogs: res
        }
      })
    },
    
    components: { BlogSection },

    transition: {
      name: 'slide-fade'
    },

    head () {
      return {
        title: this.$t('indexPageHead.title'),
        htmlAttrs: {
          lang: this.$i18n.locale,
        },
        meta: [
          { name: "author", content: "Andrew Lush" },
          { name: "description", property: "og:description", content: this.$t('indexPageHead.description'), hid: "description" },
          { property: "og:title", content: this.$t('indexPageHead.title') },
          { property: "og:image", content: this.ogImage },
          { name: "twitter:description", content: this.$t('indexPageHead.description') },
          { name: "twitter:image", content: this.ogImage }
        ]
      };
    },

    computed: {
      ogImage: function () {
        return;
      }
    }
  }
</script>

<style scoped>
  .pageHeader {
    align-content: center;
    text-align: center;
    margin: 1rem;
    width: 90%;
    font-size: 3rem;
    font-weight: 700;
  }
</style>
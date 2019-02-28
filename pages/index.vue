<template>
  <section id=blog-container>
   
    <div id="homeBanner" :style="{ backgroundImage: 'url(' + imageBanner + ')' }">
      <Nav />
      <p class="main-Page-Title">{{ pageTitle }}</p>
      <img id="down" src="../assets/images/blog/downBlue.svg" />
    </div>

    <div id="homeBannerDesk" :style="{ backgroundImage: 'url(' + largeBanner + ')' }">
      <Nav />
      <p class="main-Page-Title">{{ pageTitle }}</p>
      <img id="down" src="../assets/images/blog/downBlue.svg" />
    </div>

    <div id='dropdown'>
      <span>Viewing Posts</span>

      <div>
        <b-nav vertical class="w-25">
          <b-nav-item-dropdown
            id="nav7_ddown"
            text="All"
            extra-toggle-classes="nav-link-custom"
            left
            split
          >
            <b-dropdown-item v-for="(category, i) in blogCategories" :key='"cat" + i'>{{ category }}</b-dropdown-item>
            <b-dropdown-item active>All</b-dropdown-item>
          </b-nav-item-dropdown>
        </b-nav>
      </div>
    </div>
  
    <main id="all">

      <div id="otherPosts">

        <PostLayerImg id='blogPost1Desk' :post='blogPosts[0]' />

        <PostLayerImg id='blogPost1' :post='blogPosts[0]' />

        <BlogPost id='blogPost2' :post='blogPosts[1]' :desktopImage='desktopImages[1]'/>

        <PostLayerImg id='blogPost3' :post='blogPosts[2]' :desktopImage='desktopImages[2]'/>

        <BlogPost id='blogPost4' :post='blogPosts[3]' :desktopImage='desktopImages[3]'/>

        <PostLayerImg id='blogPost5' :post='blogPosts[4]' :desktopImage='desktopImages[4]'/>

        <BlogPost id='blogPost6' :post='blogPosts[5]' :desktopImage='desktopImages[5]'/>

        <PostLayerImg id='blogPost7' :post='blogPosts[6]' :desktopImage='desktopImages[5]'/>
      </div>

       <div id='blog-pagination'>
        <nuxt-link class="pagination-link" to="#"><img :src="otherImages[1]"/></nuxt-link>
        <nuxt-link class="pagination-link" to="#">1</nuxt-link>
        <nuxt-link class="pagination-link active" to="#">2</nuxt-link>
        <nuxt-link class="pagination-link" to="#">3</nuxt-link>
        <nuxt-link class="pagination-link" to="#">4</nuxt-link>
        <nuxt-link class="pagination-link" to="#">5</nuxt-link>
        <nuxt-link class="pagination-link" to="#"><img :src="otherImages[2]"/></nuxt-link>
      </div>
      
    </main>

   

    <section id="side-menu">
      <div id="side-menu-links">
        <nuxt-link to="/" class="side-link" v-for="(category, i) in blogCategories" :key='"blogcat" + i'>{{ category }}
        </nuxt-link>
        <nuxt-link class="side-link" to="#all">All</nuxt-link>
      </div>

      <div class='side-wrapper'>

        <form>
          <h3>{{ formText.textHeader}}</h3>
          <p>{{ formText.textBlurb }}</p>
          <input placeholder="Email" type="text" />
          <button>Subscribe</button>
        </form>

        <SideMenuItem id="sideMenuImg1" :sideMenu='sideMenu[0]' />

        <SideMenuLayerImg id="sideMenuImg2" :sideMenu='sideMenu[1]' :image='otherImages[0]' />
      
      </div>
      
      <div id="social-image-icons">
         <div class="socialIcons" v-for="(icon, i) in socialIcons" :key="'icon'+i" >
          <img :src="icon" />
        </div>
      </div>
    </section>

    <Footer />
  </section>
</template>

<script>
import largeBanner from '@/assets/images/blog/Hero-blog-desktop.png'
import Prismic from 'prismic-javascript'
import linkresolver from '../link-resolver.js'
import PrismicDOM from 'prismic-dom'
import Nav from '../components/Nav'
import Footer from '../components/Footer'
import PostLayerImg from '../components/PostLayerImg'
import BlogPost from '../components/BlogPost'
import SideMenuLayerImg from '../components/SideMenuLayerImg'
import SideMenuItem from '../components/SideMenuItem'

export default {
  components: {
    Nav,
    Footer,
    PostLayerImg,
    BlogPost,
    SideMenuLayerImg,
    SideMenuItem
  },
  data(){
    return{
      largeBanner: largeBanner
    } 
  },
  async asyncData() {
    const apiEndpoint = "https://anattaecomm.prismic.io/api/v2"

    let res = await Prismic.getApi(apiEndpoint).then( api =>{
        return api.query(Prismic.Predicates.at('my.pagetemplate.uid', 'blog'));
    })
    
        const results = res.results[0].data.body
        console.log("Documents: ", results);

        const imageBanner = res.results[0].data.imagebanner.url
        const pageTitle = res.results[0].data.page_main_title[0].text

        const formText = {
          textHeader:"",
          textBlurb: ""
        }
        formText.textHeader = results[1].items[0].form_text_header[0].text
        formText.textBlurb = results[1].items[0].form_text_blurb[0].text

        const blogPostsArray = results[0].items
        const blogCategories = []
        const blogPosts = []

        for (let i = 0; i < blogPostsArray.length; i++){
          if( !blogCategories.includes(blogPostsArray[i].blog_category[0].text) ){
            blogCategories.push(blogPostsArray[i].blog_category[0].text)
          }

          const example = {
            postCategory:"",
            postHeader:"",
            postBody:"",
            postImage:"",
            moreInfo:""
          }

          let variable = Object.create(example)
          variable.postCategory = blogPostsArray[i].blog_category[0].text
          variable.postHeader = blogPostsArray[i].blog_header[0].text
          variable.postBody = blogPostsArray[i].blog_post_body[0].text
          variable.postImage = blogPostsArray[i].blog_post_image.url
          variable.moreInfo = blogPostsArray[i].more_info[0].text
          blogPosts.push(variable)
        }
        const sideMenuArray = results[2].items
        const sideMenu = []
        for (let i = 0; i < sideMenuArray.length; i++){
          sideMenu.push(sideMenuArray[i].side_image.url)
        }

        const socialIconsArray = results[3].items
        const socialIcons = []
        for (let i = 0; i < socialIconsArray.length; i++){
          socialIcons.push(socialIconsArray[i].social_icon.url)
        }

        const largeImages = results[4].items
        const desktopImages = []
        for(let i=0; i < largeImages.length; i++){
          desktopImages.push(largeImages[i].picture.url)
        }

        const miscImages = results[5].items
        const otherImages = []

        for(let i=0; i < miscImages.length; i++){
          otherImages.push(miscImages[i].misc_image.url)
        }

        return{
          imageBanner,
          pageTitle,
          blogCategories,
          blogPosts,
          formText,
          sideMenu,
          socialIcons,
          desktopImages,
          otherImages
        }
  }     
}
</script>

<style lang="sass">
  
</style>



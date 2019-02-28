<template>
    <section id='testimonial-page'>
        <Nav />

        <h1 id='testimonial-header'>{{ header }}</h1>

        <div id='testimonial-container'>

            <div id='group'>
               
                <div v-for='(testimonial, i) in testimonials' 
                :key="'testimonial' + i" class='individual-testimony'>
                    <p class='grey'>{{ testimonial.message }}</p>
                    <div class='testimonial-person'>
                        <img :src='testimonial.image' />
                        <span>{{ testimonial.name }}</span>
                    </div>
                </div>
                
            </div>

        </div>

        <Footer />

    </section>
</template>

<script>
import Prismic from 'prismic-javascript'
import linkresolver from '../link-resolver.js'
import PrismicDOM from 'prismic-dom'
import Nav from '../components/Nav'
import Footer from '../components/Footer'

export default {
    components: {
        Nav,
        Footer
    },
    
    async asyncData() {
        const apiEndpoint = "https://anattaecomm.prismic.io/api/v2"

        let res =  await Prismic.getApi(apiEndpoint).then( api =>{
                return api.query(Prismic.Predicates.at('my.pagetemplate.uid', 'mainpage'));
                })
            
                const results = res.results[0].data.body

                const header = results[3].primary.header[0].text
                const testimonialList = results[3].items
                const testimonials = []

                for(let i=0; i < testimonialList.length; i++){
                    testimonials[i] = {
                        message: testimonialList[i].testimony[0].text,
                        name: testimonialList[i].person_name[0].text,
                        image: testimonialList[i].person_image.url,
                    }
                }  
        return{
            header,
            testimonials
        }
    } 
}
</script>

<style>

</style>

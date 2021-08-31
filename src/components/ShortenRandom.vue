<template>
    <div>
        <input id="shortenUrlInput" v-model="urlInput" placeholder="Insert your link">
        <button @click="shortenUrl()" :disabled="!urlInput">Shorten</button>
        <br/>
        <hr/>
        <div style="text-align:center;" :class="{'hide' : shortenedUrl == null , 'show' : shortenedUrl != null}">
            <a v-bind:href="'http://127.0.0.1:8000/' + shortenedUrl" target="_blank">
                <strong>http://127.0.0.1:8000/</strong>{{ shortenedUrl }}
            </a>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'ShortenRandom',
  data(){
      return{
          urlInput:'',
          info: null,
          shortenedUrl: null
      }
  },
  methods:{
        validURL(url){
            var pattern = new RegExp('^(https?:\\/\\/)?'+ // protocol
                '((([a-z\\d]([a-z\\d-]*[a-z\\d])*)\\.)+[a-z]{2,}|'+ // domain name
                '((\\d{1,3}\\.){3}\\d{1,3}))'+ // OR ip (v4) address
                '(\\:\\d+)?(\\/[-a-z\\d%_.~+]*)*'+ // port and path
                '(\\?[;&a-z\\d%_.~+=-]*)?'+ // query string
                '(\\#[-a-z\\d_]*)?$','i'); // fragment locator
            return !!pattern.test(url);
        },
        shortenUrl(){
            var url = document.getElementById('shortenUrlInput').value
            if( url.length){
                if( this.validURL(url) ){
                    if( url.indexOf("http://") == 0 || url.indexOf("https://") == 0 ){
                        //has protocol
                    } 
                    else{
                        url = 'http://' + url;
                    }
                    console.log(url);
                    
                    const reqdata = {'requestedurl':url};
                    axios
                        .post('http://127.0.0.1:8000/api/shorten',reqdata)
                        .then(response => this.shortenedUrl = response.data);
                }
            }
        }
  }/*,
  mounted () {
    axios
      .get('https://api.coindesk.com/v1/bpi/currentprice.json')
      .then(response => (this.info = response))
  }*/
}
</script>

<style scoped>
    .hide{display:none;}
</style>

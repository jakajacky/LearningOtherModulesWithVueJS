<template>
  <div>
    <mt-header fixed title="列表">
      <router-link to="/" slot="left">
        <mt-button icon="back">返回</mt-button>
      </router-link>
    </mt-header>

    <mt-navbar class="page-part" v-model="selected" :fixed=false>
        <mt-tab-item id="1">Movie</mt-tab-item>
      <mt-tab-item id="2">CNodeJS</mt-tab-item>
      <mt-tab-item id="3">Test</mt-tab-item>
    </mt-navbar>

    <!-- tab-container -->
    <mt-tab-container v-model="selected">
      <mt-tab-container-item id="1">

        <drcell :title="movie.title" v-for="(movie,index) in articles" :tags="movie.genres" :casts="movie.casts" :rate="movie.rating.average" is-link :to="'/detail/'+movie.id" :index="index">
          <img slot="icon" :src="movie.images.medium" width="70" height="88">
        </drcell>

      </mt-tab-container-item>
      <mt-tab-container-item id="2">
        <mt-cell v-for="n in 4" :title="'CNodeJS ' + n" :value="n"  is-link href="http://www.baidu.com" />
      </mt-tab-container-item>
      <mt-tab-container-item id="3">
        <mt-cell v-for="n in 6" :title="'Test ' + n" :value="n" />
      </mt-tab-container-item>
    </mt-tab-container>
    <br />
    <br />
    <br />
    <br />
  </div>
</template>

<script>
import footerTemp from '.././footerTemp.vue'
import routerTemp from '.././routerTemp.vue'
import drcell    from '.././cell/cell.vue'
import Storage from '../../storage'

import { Navbar, Tabbar, TabItem, Cell} from 'mint-ui'
import { TabContainer, TabContainerItem } from 'mint-ui'

export default {
  name: 'app',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      content:"",
      isno:0,
      input:'',
      radio:'1',
      articles:[],
      ischange:'head-title',
      isClicked:false,
      selected:'1',
      lin:'/detail/0'
    }
  },
  // 计算属性
  computed:{
    nowdate:function() {
      var datee = new Date();
      console.log('Time:'+datee.toLocaleDateString());

      return datee.toLocaleDateString();
    }
  },
  methods: {
    readyInput:function (event) {
      // msg = content
      // data.msg = event.target.value // 获取该事件event，target控件
    },
    clickBtn:function (event) {
      if (this.Clicked) {
        this.ischange = 'head-title'
      }
      else {
        this.ischange = 'head-title1'
      }
      this.Clicked = !this.Clicked
      alert(this.a)
    },
    // 监听的回调
    listenToRouterTemp: function (msg) {
      if (this.content === '') {
        this.content = msg;
      }
      else {
        this.content = '';
      }

    }
  },
  mounted:function(){
    this.articles = Storage.fetch();
    if (this.articles) {

    }
    else {
      this.$http.jsonp('https://api.douban.com/v2/movie/top250?count=10', {}, {
        header: {

        },
        emulateJSON: true
      }).then(function(response) {
        // 这里是处理正确的回调
        // console.log(this.articles);
        this.articles = response.data.subjects
        this.$nextTick(function(){
          console.log('DOM更新了'+this);
        })

      }, function(response) {
        // 这里是处理错误的回调
        console.log(response)
      });
    }

  },
  // 监听器
  watch : {
    articles: {
      handler:function (oldVal, newVal) {
        console.log('有新的数据被获取到，请持久化到本地');
        // 持久化
        Storage.save(this.articles)
      },
      deep:true
    }
  },
  components:{footerTemp,routerTemp, Tabbar, Navbar, TabItem, TabContainer, TabContainerItem, Cell,drcell},
  // 自定义指令
  // directives:{
  //   'focus':{
  //     inserted:function(el) {
  //       el.focus();
  //       console.log('focus开始了。。。。');
  //     }
  //   }
  // },
}
</script>

<template>
    <div id="container">
      <mt-header fixed title="列表">
        <router-link to="/" slot="left">
          <mt-button icon="back">返回</mt-button>
        </router-link>
      </mt-header>

      <div class="movie-detail" style="display:flex;flex-direction:row">
        <img class="img-detail" v-lazy="this.imgSrc">
        <div style="display:flex;flex-direction:column">
          <span v-text="currentArt.title" style="margin-top:6px; margin-left:6px;"></span>
          <div style="display:flex;flex-direction:row;margin-left:6px;">
            <img v-for="index in selected_stars" src="../../assets/star_selected.png" style="width:12px;height:12px;margin-left:2px;">
            <img v-for="index in half_star" src="../../assets/star_half.png" style="width:12px;height:12px;margin-left:2px;">
            <img v-for="index in unselected_stars" src="../../assets/star_deselected.png" style="width:12px;height:12px;margin-left:2px;">
            <span v-text="average" style="font-size:12px;margin-left:6px;"></span>
            <span style="font-size:12px;">分</span>
          </div>
        </div>

      </div>



    </div>
</template>

<script>
import Storage from '../../storage'
export default {

  data() {
    return {
      articles:[],
      currentId:String,
      currentArt:Object,
      imgSrc:'https://img3.doubanio.com/view/movie_poster_cover/lpst/public/p480747492.jpg',
      average:Number,
      selected_stars:Number,
      unselected_stars:Number,
      half_star:Number,
    }
  },
  ready:function() {
    console.log("路由传参：");
  },
  create:function() {
    console.log("路由传参：");
  },
  mounted:function() {
    console.log("路由传参："+this.$route.params.id);
    this.currentId = this.$route.params.id;
    this.articles = Storage.fetch();
    console.log(this.articles);
    for (var i = 0; i < this.articles.length; i++) {
      var art = this.articles[i];
      if (art.id === this.currentId) {
        this.currentArt = art;
        this.imgSrc     = this.currentArt.images.large;
        console.log(this.imgSrc);
        this.average = this.currentArt.rating.average;
        this.selected_stars = parseInt(this.currentArt.rating.stars/10);

        if (this.currentArt.rating.stars/10 !== this.selected_stars) {
          console.log('一共的星星数：'+this.selected_stars+this.currentArt.rating.stars/10);
          this.half_star = 1;
        }
        else {
          this.half_star = 0;
        }
        this.unselected_stars = 5-this.selected_stars-this.half_star;

      }
    }
  }
}

</script>

<style>
  image[lazy=loading] {
  width: 140px;
  height: 200px;
  margin: auto;
  }
  .img-detail {
    background-color: #ddd;
    width: 140px;
    height: 200px;
    margin-top: 6px;
  }
</style>

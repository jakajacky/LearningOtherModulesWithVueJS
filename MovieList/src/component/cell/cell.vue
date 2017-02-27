<template>
  <a class="drcell" :href="href">
    <span class="dr-cell-mask" v-if="isLink"></span>
    <div class="dr-cell-left">
      <slot name="left"></slot>
    </div>
    <div class="dr-cell-wrapper">
      <div class="dr-cell-title">
        <slot name="icon">
          <i v-if="icon" class="mintui" :class="'mintui-' + icon"></i>
        </slot>
        <slot name="title">
          <div class="dr-cell-contain">
            <div style="display:flex; flex-direction:row">
              <span class="dr-cell-text" v-text="title"></span>
              <span class="dr-cell-rating" v-text="rate"></span>
              <span class="dr-cell-rating-u">分</span>
            </div>

            <br/>
            <div style="display:flex; flex-direction:row">
              <span class="dr-cell-label" v-text="movieTags"></span>
            </div>
            <div style="display:flex; flex-direction:row">
              <span class="dr-cell-label1" v-text="'主演：'+movieCatas"></span>
            </div>

          </div>

        </slot>
      </div>
      <!-- <div class="dr-cell-value" :class="{ 'is-link' : isLink }">
        <slot>
          <span v-text="value"></span>
        </slot>
      </div> -->
    </div>
    <div class="dr-cell-right">
      <slot name="right"></slot>
    </div>
  </div>
  <div class="dr-cell-line">
  </div>
    <!-- <i v-if="isLink" class="dr-cell-allow-right"></i> -->
  </a>
</template>

<script>
// if (process.env.NODE_ENV === 'component') {
//   require('./style.css');
// }

/**
 * mt-cell
 * @module components/cell
 * @desc 单元格
 * @param {string|Object} [to] - 跳转链接，使用 vue-router 的情况下 to 会传递给 router.push，否则作为 a 标签的 href 属性处理
 * @param {string} [icon] - 图标，提供 more, back，或者自定义的图标（传入不带前缀的图标类名，最后拼接成 .mintui-xxx）
 * @param {string} [title] - 标题
 * @param {string} [label] - 备注信息
 * @param {boolean} [is-link=false] - 可点击的链接
 * @param {string} [value] - 右侧显示文字
 * @param {slot} - 同 value, 会覆盖 value 属性
 * @param {slot} [title] - 同 title, 会覆盖 title 属性
 * @param {slot} [icon] - 同 icon, 会覆盖 icon 属性，例如可以传入图片
 *
 * @example
 * <mt-cell title="标题文字" icon="back" is-link value="描述文字"></mt-cell>
 * <mt-cell title="标题文字" icon="back">
 *   <div slot="value">描述文字啊哈</div>
 * </mt-cell>
 */
export default {
  name: 'drcell',

  props: {
    to: [String, Object],
    icon: String,
    title: String,
    // 设置属性的 验证
    tags:{
      type:Array,
      require:true,
    },
    casts:{
      type:Array,
      require:true,
    },
    isLink: Boolean,
    value: {},
    rate:Number,
  },

  computed: {
    href() {
      if (this.to && !this.added && this.$router) {
        const resolved = this.$router.match(this.to);
        if (!resolved.matched.length) return this.to;

        this.$nextTick(() => {
          this.added = true;
          this.$el.addEventListener('click', this.handleClick);
        });
        return resolved.path;
      }
      return this.to;
    },
    // 根据电影的gener分类，计算 拼接分类 字符串
    movieTags:function() {
      var tags =  this.tags[0];
      for (var i = 1; i < this.tags.length; i++) {

         tags = tags+' / '+this.tags[i];
      }

      return tags;
    },
    // 根据电影的casts，也就是演员表，<计算> 并拼接主演字符串
    movieCatas:function() {
      var catas = this.casts[0].name;
      for (var i = 1; i < this.casts.length; i++) {

         catas = catas+' / '+this.casts[i].name;
      }

      return catas;
    }
  },
  mounted:function() {

  },

  methods: {
    handleClick($event) {
      $event.preventDefault();
      this.$router.push(this.href);
    }
  }
};
</script>

<style lang="css">

  .dr-cell {
      background-color:#fff;
      box-sizing:border-box;
      color:inherit;
      min-height:48px;
      display:block;
      overflow:hidden;
      position:relative;
      text-decoration:none;
  }
  .dr-cell img {
      vertical-align:middle;
  }
  .dr-cell:first-child .dr-cell-wrapper {
      background-origin:border-box;
  }
  .dr-cell:last-child {
      background-image:-webkit-linear-gradient(bottom, #d9d9d9, #d9d9d9 50%, transparent 50%);
      background-image:linear-gradient(0deg, #d9d9d9, #d9d9d9 50%, transparent 50%);
      background-size:100% 1px;
      background-repeat:no-repeat;
      background-position:bottom;
  }
  .dr-cell-wrapper {
      background-image:-webkit-linear-gradient(top, #d9d9d9, #d9d9d9 50%, transparent 50%);
      background-image:linear-gradient(180deg, #d9d9d9, #d9d9d9 50%, transparent 50%);
      background-size: 120% 1px;
      background-repeat: no-repeat;
      background-position: top left;
      background-origin: content-box;
      -webkit-box-align: center;
          -ms-flex-align: center;
              align-items: center;
      box-sizing: border-box;
      display: -webkit-box;
      display: -ms-flexbox;
      display: flex;
      font-size: 16px;
      line-height: 1;
      min-height: inherit;
      overflow: hidden;
      padding: 0 10px;
      width: 100%;
  }
  .dr-cell-mask {}
  .dr-cell-mask::after {
      background-color:#000;
      content:" ";
      opacity:0;
      top:0;
      right:0;
      bottom:0;
      left:0;
      position:absolute;
  }
  .dr-cell-mask:active::after {
      opacity:.1;
  }
  .dr-cell-text {
      vertical-align: top;
      margin-top: 6px;
  }
  .dr-cell-rating {
    color: orange;
    font-size: 15px;
    margin-top: 8px;
    margin-left: 6px;
  }
  .dr-cell-rating-u {
    color: orange;
    font-size: 12px;
    margin-top: 9px;
  }

  .dr-cell-label {
      color: #888;
      /*display: block;*/
      font-size: 12px;
      margin-top: 6px;
      margin-right: 2px;
  }
  .dr-cell-label1 {
      color: #888;
      /*display: block;*/
      font-size: 12px;
      margin-top: 6px;
  }
  .dr-cell-contain {
    display: flex;
    flex-direction: column;
    margin-left: 6px;
  }
  .dr-cell-title {
    display: flex;
    margin-top: 6px;
      -webkit-box-flex: 1;
          -ms-flex: 1;
              flex: 1;
              flex-direction: row;
  }
  .dr-cell-value {
      color: #888;
      display: -webkit-box;
      display: -ms-flexbox;
      display: flex;
      -webkit-box-align: center;
          -ms-flex-align: center;
              align-items: center;
  }
  .dr-cell-value.is-link {
      margin-right:24px;
  }
  .dr-cell-left {
      position: absolute;
      height: 100%;
      left: 0;
      -webkit-transform: translate3d(-100%, 0, 0);
              transform: translate3d(-100%, 0, 0);
  }
  .dr-cell-right {
      position: absolute;
      height: 100%;
      right: 0;
      top: 0;
      -webkit-transform: translate3d(100%, 0, 0);
              transform: translate3d(100%, 0, 0);
  }
  .dr-cell-allow-right::after {
      border: solid 2px #c8c8cd;
      border-bottom-width: 0;
      border-left-width: 0;
      content: " ";
      top:50%;
      right:20px;
      position: absolute;
      width:5px;
      height:5px;
      -webkit-transform: translateY(-50%) rotate(45deg);
              transform: translateY(-50%) rotate(45deg);
  }
  /* Cell Component */
  /* Header Component */
  /* Button Component */
  /* Tab Item Component */
  /* Tabbar Component */
  /* Navbar Component */
  /* Checklist Component */
  /* Radio Component */
  /* z-index */
  .dr-cell-swipe .dr-cell-wrapper, .dr-cell-swipe .dr-cell-left, .dr-cell-swipe .dr-cell-right {
      -webkit-transition: -webkit-transform 150ms ease-in-out;
      transition: -webkit-transform 150ms ease-in-out;
      transition: transform 150ms ease-in-out;
      transition: transform 150ms ease-in-out, -webkit-transform 150ms ease-in-out;
  }
  .dr-cell-swipe-buttongroup {
      height: 100%;
  }
  .dr-cell-swipe-button {
      height: 100%;
      display: inline-block;
      padding: 0 10px;
      line-height: 48px;
  }

  .dr-cell-line {
    height: 1px;
    background-color: lightgray;
    margin-top: 6px;
  }
</style>

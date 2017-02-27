<template>
    <div>
      <p>详情页面</p>
    </div>
</template>

<script>
import Storage from '../../storage'
export default {
  data() {
    return {
      articles:[],
    }
  },
  mounted:function() {
    this.articles = Storage.fetch();
    console.log(this.articles);
  }
}

</script>

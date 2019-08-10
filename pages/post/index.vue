<template>
  <!-- <p>文章首页</p> -->
  <div class="post">
    <el-row>
      <el-col :span="6">
        <postAside @tuijiancity="searchcity"></postAside>
      </el-col>
      <el-col :span="18">
        <postmain :data="data" @city="searchcity" @pagenum='pagenum' :city='city'></postmain>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import postAside from "@/components/post/postAside";
import postmain from "@/components/post/postmain";
export default {
  data() {
    return {
      data: {
        data: []
      },
      limit: 3,
      start: 0,
      city:''
    };
  },
  methods: {
    searchcity(city) {
      if(city){
        this.city = city
      }
      
      let params = {
        _start: this.start, // 索引
        _limit: this.limit // 数量
      };
      if (this.city) {
        params.city = city;
      }
      this.$axios({
        url: "/posts",
        params
      }).then(res => {
        this.data = res.data;
        this.data.data.forEach(item => {
          if (item.images.length > 3) {
            item.images.length = 3;
          }
        });
        console.log(res.data)
      });
    },

    pagenum(num,size){
      this.start = (num-1)*size
      this.limit = size
      this.searchcity(this.city)
    }
  },
  components: {
    postAside,
    postmain
  },
  mounted() {
    this.searchcity();
  }
};
</script>

<style scoped lang='less'>
.post {
  width: 1000px;
  margin: 0 auto;
}
</style>

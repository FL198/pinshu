<template>
  <div>
    <mt-header
      :title="title"
      class="book-header"
    >
      <div
        slot="left"
        @click="$router.back(-1)"
      >
        <mt-button icon="back">
          返回
        </mt-button>
      </div>
    </mt-header>
    <div class="container">
      <div class="blank" />
      <div
        v-for="(item,idx) in booksList"
        :key="item.id"
        class="bookList"
        @click="goDetail(item._id)"
      >
        <div class="images">
          <img :src="booksList[idx].cover | setCover">
        </div>
        <div class="book-message">
          <h4>{{ booksList[idx].title }}</h4>
          <div class="book-info mui-ellipsis-2">
            {{ booksList[idx].shortIntro }}
          </div>
          <div class="author-cate">
            <div class="auImage">
              <img src="../../assets/user.png">
            </div>
            <div class="author">
              {{ booksList[idx].author }}
            </div>
            <div class="majorCate">
              {{ booksList[idx].majorCate }}
            </div>
            <div class="retentionRatio">
              留存率:{{ booksList[idx].retentionRatio }}%
            </div>
          </div>
        </div>				
      </div>
    </div>
  </div>
</template>

<script>
import myaxios, {rankingURL, staticPath} from '@/tools/myaxios.js'
export default{
    filters: {
        setCover(cover) {
            if(cover.indexOf(staticPath) > -1) {
                return cover
            }
            return staticPath + cover
        }
    },
    data: function(){
        return {
            booksList: [],
            id: [],
            flag: false,
            isBack: false,
            title: "女生排行"
        }
    },
    async created(){

        //			console.log(this.$route.params.id);
        let data = await myaxios("GET", rankingURL + this.$route.params.id)

        //			console.log(data);
        this.booksList = data.data.ranking.books

        // console.log(this.booksList)
    },
    methods: {
        goback() {
            this.isBack = true
            this.$router.go(-1)
        },
        goDetail(id){
            this.$router.push({
                path: "/femalerankDetail/femaleBooksInfo",
                query: {id: id}
            })
        }
    }

}
</script>

<style lang="less">
	* {
		margin: 0;
		padding: 0;
	}
	
	header {
		width: 100%;
		height: 40px;
		background-color: #C62F2F;
		color: white;
		text-align: center;
		line-height: 40px;
		font-size: 18px;
		font-family: "微软雅黑";
		position: fixed;
		left: 0;
		top: 0;
		.mint-header-title {
			position: absolute;
			left: 160px;
			bottom: 0;
			line-height: 34px;
		}
	}

	.blank{
		width: 100px;
		height: 1px;
	}
	.container{
		width: 100%;
		height: 100%;
		// margin-top: 40px;
		margin-bottom: 50px;
	}
	.bookList{
		
		width: 100%;
		height: 110px;
		/*border: 1px solid red;*/
		margin-top: 14px;
		.images{
			float: left;
			margin-left: 10px;
			width: 80px;
			height: 110px;
			img{
				width: 80px;
				height: 110px;
			}
			
		}
		.book-message{
			float: left;
			width: 68%;
			height: 110px;
			margin-left: 10px;
			h4{
				font-size: 16px;
				font-weight: 700;
				font-family: "微软雅黑";
				margin-top: 4px;
				
			}
			.book-info{
				width: 95%;
				height: 40px;
				font-size: 14px;
				font-family: "微软雅黑";
				color: gray;
				overflow: hidden;
				/*background-color: pink;*/
				margin-top: 15px;
			}
		}
		.author-cate{
			width: 100%;
			height: 20px;
			margin-top: 14px;
			.auImage{
				float: left;
				width: 15px;
				height: 20px;
			}
			img{
				width: 15px;
				height: 15px;
				margin-top: 2px;
			}
			.author{
				float: left;
				width: 60px;
				height: 15px;
				color: gray;
				font-size: 12px;
				font-family: "微软雅黑";
				overflow: hidden;
			}
			.majorCate{
				float: left;
				width: 50px;
				height: 20px;
				background-color: #ccc;
				font-size: 12px;
				color: gray;
				font-family: "微软雅黑";
				border-radius: 5px;
				text-align: center;
				line-height: 20px;
				margin-left: 6%;
				
			}
			.retentionRatio{
				float: left;
				width: 90px;
				height: 20px;
				background-color: #C0A16B;
				font-size: 12px;
				color: red;
				font-family: "微软雅黑";
				border-radius: 5px;
				text-align: center;
				line-height: 20px;
				margin-left: 5px;
			}
		}
	}
</style>
<template>
    <div id="app">
        <div class="left-nav">
            <div class="head-portrait">
                <img src="./assets/head-portrait.jpg" height="210" width="210" @click="headPortrait">
            </div>
            <div class="name">
                <h3>{{name}}</h3>
            </div>
            <div class="underline"></div>
            <div class="introduce">
                <p>{{individualitySignature}}</p>
            </div>
            <div class="archive">
                <div class="archive-left" @click="article">
                    <el-tooltip class="item" effect="dark" content="文章" placement="bottom">
                        <p>{{articleNumber}}</p>
                        <img src="./assets/file.png" height="20" width="20">
                    </el-tooltip>
                </div>
                <div class="divider"></div>
                <div class="archive-center" @click="classification">
                    <el-tooltip class="item" effect="dark" content="分类" placement="bottom">
                        <p>0</p>
                        <img src="./assets/folder.png" height="20" width="20">
                    </el-tooltip>
                </div>
                <div class="divider"></div>
                <div class="archive-right" @click="label">
                    <el-tooltip class="item" effect="dark" content="标签" placement="bottom">
                        <p>{{tagNumber}}</p>
                        <img src="./assets/bookmark.png" height="20" width="20">
                    </el-tooltip>
                </div>
            </div>
            <div class="nav-router">
                <ul>
                    <router-link to="/home"><li>首页</li></router-link>
                    <router-link to="/archive"><li>归档</li></router-link>
                    <router-link to="/about"><li>关于我</li></router-link>
                    <div class="search" @mouseover="searchover" @mouseout="searchout">
                        <el-input 
                            class="search-input"
                            v-show="isShowSeach"
                            placeholder="请输入标题关键字"
                            icon="search"
                            v-model="seachTitle">
                        </el-input>
                        <span v-show="!isShowSeach">搜索</span>
                    </div>
                </ul>
            </div>
        </div>
        <div class="content-panel">
            <transition name="fade" mode="out-in">
                <router-view></router-view>
            </transition>
        </div>
    </div>
</template>

<script>
export default {
    name: 'app',
    data () {
        return {
            isShowSeach: false,
            seachTitle: '',
            name: '',
            individualitySignature: '',
            tagNumber: '',
            articleNumber: ''
        }
    },
    mounted: function(){
         this.$http.get('/api/personalInformation').then(
              respone => {
                   this.name = respone.body[0].name,
                   this.individualitySignature = respone.body[0].individualitySignature
              },
              respone => console.log(respone)
         )
         this.$http.get('/api/getArticleLabel').then(
              respone => this.tagNumber = respone.body.length,
              respone => console.log(respone)
         )
         this.$http.get('/api/articleList').then(
    		  respone => this.articleNumber = respone.body.length,
    		  respone => console.log(respone)
    	 )
    },
    methods: {
        headPortrait: function(){
            this.$router.push('/about')
        },
        // 文章
        article: function(){
            this.$router.push('/home')
        },
        // 分类
        classification: function(){
            this.$router.push('/classify')
        },
        //标签
        label: function(){
            this.$router.push('/label')
        },
        searchover: function(){
            this.isShowSeach = true
        },
        searchout: function(){
            this.isShowSeach = false
        }
    },
    directives: {
        contentHight: {
            bind: function(el){
                var contentHeight = window.innerHeight - 100
                el.style.height = contentHeight + 'px'
            }
        }
    }
}
</script>

<style>
.fade-enter-active, .fade-leave-active {
    transition: opacity .5s
}
.fade-enter, .fade-leave-active {
    opacity: 0
}
pre {
    background-color: #f5f5f5;
    padding: 0.5rem 1rem;
    border-radius: 3px;
    margin: 10px 0 10px 0;
    overflow-x: auto;
}
blockquote {
    border-left: 10px #e4e9eb solid;
    background-color: #f5f5f5;
    padding: 15px 20px;
    margin: 10px 0 10px 0;
}
strong {
    color: #579eff;
}
* {
    margin: 0;
    padding: 0;
    list-style: none;
    text-decoration: none;
    font-faminly: Source Sans Pro,Helvetica Neue,Arial,sans-serif;
}
a:visited, a:link, a:active {
    color: #545455;
    cursor: pointer;
}
body {
    background-color: #f5f5f5;
}
#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    width: 100%;
    height: 100%;
    margin: 20px 50px 0 50px;
}
.left-nav {
    width: 250px;
    height: 493px;
    background-color: #fff;
    border-radius: 5px;
    float: left;
    box-shadow: 0 0px 5px 0px #dadada;
    text-align: center;
}
.left-nav:hover .underline{
    width: 0px;
    transition: all 0.5s; 
}
.head-portrait {
    width: 100px;
    height: 100px;
    border-radius: 50%;
    overflow: hidden;
    margin: 0 auto;
    margin-top: 20px;
    margin-bottom: 20px;
    cursor: pointer;
}
.name {
    color: #545455;
    margin: 0 auto;
    width: 40%;
}
.underline {
    width: 100px;
    height: 1px;
    background-color: #bababa;
    margin: 0 auto;
    margin-bottom: 10px;
    transition: all 0.5s; 
}
.introduce {
    color: #545455;
    margin-bottom: 20px;
}
.archive {
    width: 70%;
    margin: 0 auto;
    color: #666666;
    margin-bottom: 20px;
    cursor:pointer;
    position: relative;
}
.divider {
    width: 1px;
    float: left;
    height: 45px;
    background-color: #f4f4f4;
}
.archive-left {
    width: 32%;
    float: left;
    transition: all 0.5s; 
}
.archive-left:hover{
    transform: scale(1.2);
    transition: all 0.5s;
}
.archive-left:hover .tooltip{
    display: block;
}
.archive-center {
    width: 32%;
    float: left;
    transition: all 0.5s; 
}
.archive-center:hover {
    transform: scale(1.2);
    transition: all 0.5s;  
}
.archive-right {
    width: 32%;
    float: left;
    transition: all 0.5s; 
}
.archive-right:hover {
    transform: scale(1.2);
    transition: all 0.5s;  
}
.nav-router {
    clear: both;
    width: 100%;
    height: 200px;
    color: #545455;
    cursor:pointer;
    margin-top: 90px;
}
.nav-router > ul li {
    height: 50px;
    line-height: 50px;
    transition: all 0.5s;
    border-top: 1px #f3f3f3 solid; 
}
.nav-router > ul li:hover {
    background-color: #d4d4d4;
    color: #fff;
    transition: all 0.5s;
}
.search {
    height: 50px;
    line-height: 50px;
    border-top: 1px #f3f3f3 solid;
}
.search .search-input {
    margin: 0 auto;
    margin-top: 7px;
    width:220px;
    height:38px;
}
.search:hover {
    background-color: #fff;
    border-radius: 0px 0px 5px 5px;
}
.content-panel {
    width: 65%;
    background-color: #fff;
    border-radius: 5px;
    margin-left: 20px;
    float: left;
    box-shadow: 0 0px 5px 0px #dadada;
}
.content-panel-header {
    height: 40px;
    line-height: 40px;
    background-color: #fafafa;
    border-radius: 5px 5px 0 0;
    padding-left: 25px;
    box-shadow: 0 0 1px 0px #ccc;
    color: #545455;
    font-size: 20px;
}
.content-panel-content {
    min-height: 600px;
    padding: 10px 10px 10px 10px;
}
</style>







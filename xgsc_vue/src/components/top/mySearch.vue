<template>
  <div id="mysearch">
    <div class="list">
      <span class="iconfont isicon" :class="isList?'icon-caidan':'icon-chacha'" @click="isLists"></span>
      <!-- 搜索框+点击弹出背景 -->
      <van-cell style="marign:0px;" @click="showPopup">
        <mt-search class="search" 
                v-model="value"
                cancel-text="" placeholder="搜索商品..."
                >
            <!-- <mt-cell>
            </mt-cell> -->
        </mt-search>  
      </van-cell>
      <span class="submit" @click="searchSubmit">搜索</span>
      <!-- -------- -->
      <span class="iconfont icon-lingdang isicon"></span>
       <!-- 弹出背景内容设置 -->
      <van-popup position="top" class="popup_bg"
              :style="{ height: '100%' ,top:'3.3rem'}"
              v-model="show" close-icon-position="top-right"
              closeable>
        <p><router-link to="/"><img src="/img/icon/xiaogou.png"></router-link></p>
        <p>热门搜索<span><img src="/img/icon/timg.gif"></span></p>
        <ul>
          <!-- 待跳转 -->
          <li><router-link to="/details?id=1">T10 Plus</router-link></li>
          <li><router-link to="/details?id=2">R55 Pro</router-link></li>
          <li><router-link to="/details?id=3">D-620 Alr</router-link></li>
          <li><router-link to="/details?id=4">T10</router-link></li>
        </ul>
        <p class="res" v-show="isCount">搜索-<span>{{value?value:'全部'}}</span> 共有：<span>{{count}} &nbsp;</span>个结果</p>
          <!-- 搜索结果列表 -->
        <div class="searchList">
          <ul>
            <li v-for="(res,i) of searchRes" :key="i">
              <router-link :to="{path:'/details',query:{id:res.pid}}">
                <img :src="res.src">
                <p v-text="res.pname"></p>
                <span v-text="`¥  ${res.price}`"></span>
              </router-link>
            </li>
          </ul>
        </div>
      </van-popup>
     <!-- --------- -->
    </div>
    <!-- 手风琴-->
    <div class=""  :class="isHidden?'unlist':'enlist'">
      <van-collapse v-model="activeName" accordion>
        <van-collapse-item title="产品指南" name="1">
          <ul>
            <li>桌面吸尘器</li>
            <li>手持/推杆</li>
            <li>除螨仪</li>
            <li>卧式吸尘器</li>
            <li>扫地机</li>
            <li>桶式</li>
            <li>配件</li>
          </ul>
        </van-collapse-item>
        <van-collapse-item title="核心技术" name="2">
        </van-collapse-item>
        <van-collapse-item title="中央维修" name="3"></van-collapse-item>
        <van-collapse-item title="媒体中心" name="4"></van-collapse-item>
        <van-collapse-item title="关于我们" name="5">
          <ul>
            <li>公司介绍</li>
            <li>招商合作</li>
            <li>奖项专利</li>
            <li>社会责任</li>
          </ul>
        </van-collapse-item>
      </van-collapse> 
          
    </div>
    <!-- //////// -->
  </div>
</template>
<script>
export default {
  data(){
    return{
      show: false,//弹框
      isList:true,
      activeName: '',
      isHidden:false,
      value:'',
      searchRes:[],//保存搜索结果
      count:0,//搜索的数量
      isCount:false//默认隐藏数量
    }
  },
  methods:{
    isLists(){
      console.log(11)
      if(this.isList){
        this.isList=false,
        this.isHidden=true
      }else{
        this.isList=true,
        this.isHidden=false
      }
    },
     //弹框
    showPopup() {
      // if(this.show){
        // this.show = false
      // }else{
        this.isList = true,
        this.isHidden = false
        this.show = true;
        this.isCount = false;
        this.searchRes = null
      // }
    },
    searchSubmit(){//搜索
      this.count = "0";
      this.isCount=true;
      var value = (this.value).trim()
      console.log(value)
      this.axios.get('/search?title='+value).then(result=>{
        let res = result.data;
        if(res.code == 1){
          this.searchRes = res.searchs;
          this.count = res.searchs.length;
          console.log(res.searchs);
        }
      })
    }
  }
}
</script>
<style lang="scss">
#mysearch{
  width: 100%;
  display: block;
  margin: 0px auto;
  position: relative;
  background-color: white;
  .search{//  搜索样式设置
     height: 2.75rem;width:100%;
     height: 44px;
    .mint-searchbar{
      background-color:white;
      padding: 0.35rem 0rem;
      // left: 10%;
      .mint-searchbar-inner{
        background-color: #f2f2f2;
        border-radius: 1.125rem;
        i{font-size: 1rem;margin: 0px 0.3125rem;}
      }
      .mint-searchbar-core{background-color: #f2f2f2;}
    }
    // .mint-search-list{width: 640px; margin: 0px auto;}
  }

  // 菜单样式和右边
  .list{
    //修改顶部两边icon图标隐藏-----//
    // .isicon{display: none;}
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0px 8px;
    .van-cell{//搜索框右padding
      padding: 0.3125rem 0.375rem;
    }
    .submit{//提交搜索
      width: 35px;
      display: inline-block;
      font-size: 0.5125rem;
      // padding-left: 0.25rem;
    }
    >.iconfont{
      width: 28px;height:28px;
      color: #666;
      font:400 28px 微软雅黑;
    }
    //弹出背景设置
    .popup_bg{
      >ul{
        padding: 0.5rem 1.6875rem 0;border-bottom: 0.0325rem solid #e9e9e9;
        height: 5rem;
        li{
          float: left;margin:0rem 1.25rem 0.75rem 0;
          a{
            display: block;
            border: 1px solid rgba(221, 221, 221, 0.534);
            border-radius: 13px;
            font-size: 13px;
            padding: 7px 15px;
            color: #888;
          }
        }
      }
      p:first-child{//狗狗log
        img{width: 3rem;height: 1.75rem;margin-left: 1.25rem;}
      }
      p:nth-child(2){//热门
        font-size: 13px;
        color: #999;
        line-height: 19px;
        padding: 0.45rem 1.5rem;
        img{width: 5.5rem;height: 2.575rem;} 
      }
        //搜索结果样式----
      p:nth-child(4){
          padding: 0.3rem 0rem 0rem 1.5rem;
          height: 1rem;line-height: 1rem;
          color: #555;
          font-size: 0.85rem;
          span:first-child{color: rgba(236, 130, 8, 0.932);font-weight: 400;}
          span:last-child{color: rgb(165, 65, 29);font-weight: 700;}
      }
       //----
      .searchList{
        >ul{
          // padding: 0px 0px;
          display: block;
          display: flex;
          flex-wrap: wrap;
          width: 100%;
          li{
            width: 33%;
            text-align: center;
            a{
              padding: 0px;
              display: block;
              img{
                width: 100%;
              }
              >p{
                padding: 0rem;
                height: 20px;line-height: 20px;
                overflow: hidden;
              }
              span{
                font-size: 0.875rem;
                color: #ec232b;
                font-weight: 700;
                height: 1.25rem;line-height: 1.25rem;
                padding-bottom: 10px;
              }
            }            
          }
        }
      }

    }
  }
  .unlist{//控制菜单显示
    position: absolute;
    top: 3.1rem;left: 0rem;
    width: 100%;
    height: 40.75rem;
    z-index: 2500;
    background-color: rgb(255, 255, 255);
    // transition:9s;
    .van-collapse-item__content{
      background-color: #f8f8f8;
      padding: 1.875rem 1.25rem 1.25rem 1.25rem;
    }
    ul{
      // background-color: #f8f8f8;
      padding: 1.875rem,1.25rem,0rem,1.25rem;
      li:not(:last-child){margin-bottom: 1.875rem;}  
    }
  }
  .enlist{//控制菜单隐藏
    display: none;height: 0%;
    // transition:9s;
  }
  .van-overlay{top:6.6rem}
}
</style>
<template>
  <div id="slist"  v-loading="loading" >
      <div class="items">
          <a class="item" v-for="(item,index) in slist" @click="toSong(item.id)">
              <div class="item-num">
                  {{++index}}
              </div>
              <div class="item-main">
                  <div class="im-left">
                      <p class="title">{{item.name}}</p>
                      <p class="author">
                          <span v-for="item in item.ar" class="m-song-author">{{item.name}}</span>
                          {{'-'+item.al.name}}</p>
                  </div>
                  <div class="im-right">

                  </div>
              </div>
          </a>
      </div>
  </div>
</template>
<script>
export default {
  props: ['listId','type'],
  data(){
      return{
          slist: [],
          loading: true
      }
  },
  created(){
      this.initData(this.listId)
  },
  methods:{
      async initData(payload){
        let res = await this.getData('querySongListDetail', { 'id': payload });
        this.slist = res.data.playlist.tracks
        this.loading = false;
    },
      toSong(id){
          this.$router.push({ name: 'song', params: { 'id': id }})
      }
  },
}
</script>
<style lang="less" scoped>
@import url('../style/common.less');
#slist{
    min-height: 1rem;
    .item{
        display: flex;
        justify-content: flex-start;
        align-items: center;
        list-style: none;
        color: black;
        background-color: #fcfcfd;
        &:nth-child(-n+3) .item-num{
            color: @mcolor;
        }
        .item-num{
            padding: 0 .1rem;
            width: .3rem;
            text-align: center;
        }
        .item-main{
            display: flex;
            justify-content: space-between;
            width: 100%;
            border-bottom: 1px solid rgba(0,0,0,.1);
            padding: .05rem 0;
            .im-left{
                width: 90%;
                p{
                    margin: .03rem;
                    display: -webkit-box;
                    -webkit-line-clamp: 1;
                    -webkit-box-orient: vertical;
                    overflow: hidden;
                }
                .m-song-author{
                    &::after{
                        content: ' / ';
                    }
                    &:last-child::after{
                        content: '';
                    }
                }
                .author{
                    font-size: .14rem;
                }
            }
            .im-right{
                width:  .4rem;
                height: .5rem;
                background-image: url(../assets/index_icon_2x.png);
                background-size:2rem 1.2rem;
                background-position:15% -18%;
            }
        }
    }
}
</style>

<template>
  <div>
    <ul class="fly-list">
      <li v-for="(item,index) in items" :key="'listitem' + index">
        <router-link class="fly-avatar" :to="{name: 'home', params: {uid: item.uid._id}}" link>
          <!-- <img :src="item.uid.pic ? item.uid.pic : '/img/header.jpg'" alt="贤心" /> -->
          <img src="https://tva1.sinaimg.cn/crop.0.0.118.118.180/5db11ff4gw1e77d3nqrv8j203b03cweg.jpg">
        </router-link>
        <h2>
          <a class="layui-badge">{{item.catalog}}</a>
          <router-link :to="{name: 'detail', params: {tid: item._id}}">{{item.title}}</router-link>
        </h2>
        <div class="fly-list-info">
          <router-link :to="{name: 'home', params: {uid: item.uid._id}}" link>
            <cite>{{item.uid.name}}</cite>
            <!--<i class="iconfont icon-renzheng" title="认证信息：XXX"></i>-->
            <i
              class="layui-badge fly-badge-vip"
              v-if="item.uid.isVip !== '0'"
            >{{'VIP' + item.uid.isVip}}</i>
          </router-link>
          <span>{{item.created | moment}}</span>

          <span class="fly-list-kiss layui-hide-xs" title="悬赏飞吻">
            <i class="iconfont icon-kiss"></i>
            {{item.fav}}
          </span>
          <span class="layui-badge fly-badge-accept layui-hide-xs" v-show="item.status !== '0'">已结</span>
          <span class="fly-list-nums">
            <i class="iconfont icon-pinglun1" title="回答"></i>
            {{item.answer}}
          </span>
        </div>
        <div class="fly-list-badge" v-show="item.tags.length > 0 && item.tags[0].name !== ''">
          <span
            class="layui-badge"
            v-for="(tag, index) in item.tags"
            :key="'tag' + index"
            :class="tag.class"
          >{{tag.name}}</span>
        </div>
      </li>
    </ul>
    <div style="text-align: center" v-show="isShow">
      <div class="laypage-main" v-if="!isEnd">
        <a @click.prevent="more()" class="laypage-next">更多求解</a>
      </div>
      <div class="nomore gray" v-else>没有更多了</div>
    </div>
  </div>
</template>

<script>
import moment from "moment";
export default {
  name: 'listItem',
  props: {
    lists: {
      default: () => [],
      type: Array
    },
    isShow: {
      default: true,
      type: Boolean
    },
    isEnd: {
      default: false,
      type: Boolean
    }
  },
  computed: {
    items() {
      return this.lists
    }
  },
  methods: {
    more() {

    }
  },
  filters: {
    moment(date) {
      //超过7天显示日期
      if (moment(date).isBefore(moment().subtract(7, 'days'))) {
        return moment(date).format('YYYY-MM-DD')
      } else {//显示xx 天前
        return moment(date).from(moment())
      }

    }
  }
}
</script>

<style lang="sass" scoped>
</style>
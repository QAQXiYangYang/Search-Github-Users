<template>
  <div>
    <h2 v-show="firstView">Enter name to search</h2>

    <h2 v-show="loading">
      <Spin fix>
        <Icon type="load-c" size=18 class="demo-spin-icon-load"></Icon>
        <div>Loading</div>
      </Spin>
    </h2>

    <h2 v-show="error">{{error}}</h2>

    <div class="row" v-show="users">
      <div class="card" v-for="user in users">
        <a :href="user.html_url" target="_blank">
          <img :src="user.avatar_url" style='width: 100px' />
        </a>
        <p class="card-text" v-html="user.login">{{user.login}}</p>
      </div>
    </div>

    <!--分页器-->

    <div class="pull-right">
      <Page :total="pagecount" :page-size="pageSize" @on-change="pagechange" v-show="users" show-elevator></Page>
    </div>

  </div>
</template>
<script>

import axios from 'axios'
//定义一个变量来接收返回的数据
let items = null
export default {
  props: {
    searchName: {
      type: String
    }
  },
  data() {
    return {
      firstView: true,
      loading: false,
      pagecount: 10,
      pageSize: 6,
      users: null,
      error: null,


    }
  },
  watch: {

    searchName: function(value) {
      if (!value) {
        return
      }
      this.firstView = false
      this.loading = true
      // 发送ajax请求, 更新界面
      console.log('发送请求', value)
      const url = `https://api.github.com/search/users?q=${value}`
      axios.get(url).then((response) => {
        console.log(response)
        this.loading = false


        //切割数据  处理分页逻辑
        items = response.data.items

        for (var i = 0; i < items.length; i++) {
          if (items[i].login.indexOf(this.searchName) == 0) {
            items[i].login = items[i].login.replace(this.searchName, '<span style="background: #ffff00;">' + this.searchName + '</span>');
          }
        }


        this.pagecount = items.length
        if (items.length < this.pageSize) {
          this.users = items;
        } else {
          this.users = items.slice(0, this.pageSize);
        }

      }, (response) => {
        console.log(response)
        this.loading = false
        this.error = response.statusText
      })

    },
  },

  methods: {
    //配置分页器
    pagechange(index) {
      var _start = (index - 1) * this.pageSize;
      var _end = index * this.pageSize;
      this.users = items.slice(_start, _end);
    }
  }
}

</script>

<style>
.card {
  float: left;
  width: 33.333%;
  padding: .75rem;
  margin-bottom: 2rem;
  border: 1px solid #efefef;
  text-align: center;
}

.card>img {
  margin-bottom: .75rem;
  border-radius: 100px;
}

.card-text {
  font-size: 85%;
}

.demo-spin-icon-load {
  animation: ani-demo-spin 1s linear infinite;
}

@keyframes ani-demo-spin {
  from {
    transform: rotate(0deg);
  }
  50% {
    transform: rotate(180deg);
  }
  to {
    transform: rotate(360deg);
  }
}

.demo-spin-col {
  height: 100px;
  position: relative;
  border: 1px solid #eee;
}
</style>

<template>
  <div>
    <div class="search">
    <!-- 实现自动补全-->
      <AutoComplete
        v-model="searchName"
        :data="dataSearch"
        @on-search="handleSearch"
        placeholder="Enter the name you search"
        style="width:200px"></AutoComplete>
      <Button type="primary"  icon="ios-search" @click="handleClick">搜索</Button>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
    props: ['setSearchName'],

    data() {
      return {
        searchName: '',
        dataSearch: []
      }
    },

    methods: {
      handleClick() {
        const searchName = this.searchName && this.searchName.trim()
        if (searchName) {
          this.setSearchName(this.searchName)
          this.searchName = ''
        }
      },
      handleSearch (value) {

        //定义一个生成随机字母或者数字的方法
        function randomString(len) {
          len = len || 32;
          var $chars = 'ABCDEFGHJKMNPQRSTWXYZabcdefhijkmnprstwxyz2345678';    /****默认去掉了容易混淆的字符oOLl,9gq,Vv,Uu,I1****/
          var maxPos = $chars.length;
          var pwd = '';
          for (var i = 0; i < len; i++) {
            pwd += $chars.charAt(Math.floor(Math.random() * maxPos));
          }
          return pwd;
        }


        //定义suggest的待选内容
        this.dataSearch = !value ? [] : [
          value,
          value +randomString(1),
          value + randomString(2)
        ];
      }
    }
  }
</script>

<style>

</style>

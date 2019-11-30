<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <HelloWorld :msg="title" />
    <hr>
    <div>
      <div>
        <label>课程名称</label><input type="text" v-model="courseInfo.courseName">
      </div>
      <div>
        <label>课程价格</label><input type="number" v-model="courseInfo.coursePrice"></div>
      <div><button @click="addCourseBtn">添加到课程列表</button></div>
    </div>

    <div>
      <h3>课程列表</h3>
      <table>
        <thead>
          <tr>
            <td>课程名称</td>
            <td>价格</td>
            <td>添加都购物车</td>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item,index) in courseList" :key="item.index">
            <td>{{item.courseName}}</td>
            <td>{{item.coursePrice}}</td>
            <td><button @click="addShopCart(index)">添加到购物车</button></td>
          </tr>
        </tbody>
      </table>
    </div>
    <div>
      <shop-cart :sourseItem="sourseItem" @itemRemove="remove"/>
    </div>
  </div>
</template>

<script>
  const console = window.console; //ESlist 不允许使用没有定义的变量
  import HelloWorld from './components/HelloWorld.vue'
  import ShopCart from './components/ShopCart'
  export default {
    name: 'app',
    components: {
      HelloWorld,
      ShopCart
    },
    data() {
      return {
        title: '开课吧购物车',
        courseList: [{
            id: 0,
            courseName: 'Python',
            coursePrice: 500
          },
          {
            id: 1,
            courseName: 'Java',
            coursePrice: 800
          }
        ],
        courseInfo: {
          courseName: '',
          coursePrice: ''
        },
        sourseItem: []
      }
    },
    methods: {
      addCourseBtn() { //添加课程列表
        let item = this.courseInfo;
        var len = this.courseList.length
        this.courseList.push({
          ...item,
          id: len
        });
        this.courseInfo.courseName ='';
        this.courseInfo.coursePrice ='';
      },
      addShopCart(i) { //添加到购物车
        console.log(i)
        let item = this.courseList[i];
        var isBe = this.sourseItem.find(x => x.id == item.id);//判断是否已经添加了门课
        /**
         * 数组实例的find方法，用于找出第一个符合条件的数组成员。
         * 它的参数是一个回调函数，所有数组成员依次执行该回调函数，
         * 直到找出第一个返回值为true的成员，然后返回该成员。
         * 如果没有符合条件的成员，则返回undefined。
         */
        if (isBe) {
          isBe.num += 1;
        } else {
          this.sourseItem.push({
            ...item,
            num: 1,
            isActive: true
          });
        }
      },remove(i){
        this.sourseItem.splice(i,1)
      }
    }
  }
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
<template>
    <div>
        <h3>购物车</h3>
        <table>
            <thead>
                <tr>
                    <td>勾选</td>
                    <td>课程名称</td>
                    <td>课程价格</td>
                    <td>数量</td>
                    <td>价格</td>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(item,index) in sourseItem" :key="index">
                    <td>
                        <input type="checkbox" v-model="item.isActive">
                    </td>
                    <td>{{item.courseName}}</td>
                    <td>{{item.coursePrice}}</td>
                    <td><button @click="minus(index)">-</button>{{item.num}}<button @click="add(index)">+</button></td>
                    <td>{{item.coursePrice * item.num}}</td>
                </tr>
                <tr>
                    <td></td>
                    <td colspan="2">{{isActiveSourse}}/{{allActiveSourse}}</td>
                    <td colspan="2">{{allActivePrice}}</td>
                </tr>
            </tbody>
        </table>
    </div>

</template>

<script>
    /*因为vue是单向数据里 不推荐在子组件里修改 props */
    /*子组件如需修改props时候，需要使用$emit 父组件来监听on事件 再让父组件进行修改*/
    export default {
        name: 'shop-cart',
        props: ['sourseItem'],
        methods: {
            minus(index) {
                if (this.sourseItem[index].num == 1) {
                    if (window.confirm('是否删除？')) {
                        this.$emit('itemRemove', index);
                    }
                } else {
                    this.sourseItem[index].num--;

                }
            },
            add(index) {
                this.sourseItem[index].num++
            }
        },
        computed: { //计算属性，监听值
            isActiveSourse() { //选中的项
                var isActive = this.sourseItem.filter(x => x.isActive == true);
                //.filter() 方法的参数是一个函数，所有数组成员依次执行该函数，返回结果为true的成员组成一个新数组返回。该方法不会改变原数组。
                return isActive.length;
            },
            allActiveSourse() { //全部的项
                return this.sourseItem.length;
            },
            allActivePrice() { //全部的价格
                let j = 0;
                this.sourseItem.map(i => {
                    if (i.isActive) {
                        j += i.coursePrice * i.num
                    }
                })
                return j;
            },
        }
    }
</script>
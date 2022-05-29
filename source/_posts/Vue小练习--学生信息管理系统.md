---
title: Vue小练习--学生信息管理系统
tags:
  - Vue
  - HTML
  - CSS
  - JS
categories: Code
abbrlink: 34737
date: 2021-03-07 00:00:00
---
使用Vue实现对数据的增删改查
<!--more-->

```xml
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <script src="../Vue/vue.js"></script>
  </head>
  <style>
    * {
      margin: 0;
      padding: 0;
    }
    #example {
      width: 800px;
      margin: 50px auto;
    }
    .bkcolor {
      background-color: blueviolet;
    }
    form {
      width: 100%;
      display: flex;
      justify-content: space-between;
    }
  </style>
  <body>
    <div id="example">
      <form v-show="isShow" :class="['bkcolor']">
        <input type="text" placeholder="序号" v-model="person.num" />
        <input type="text" placeholder="姓名" v-model="person.name" />
        <input type="text" placeholder="分数" v-model="person.score" />
        <input type="submit" value="新增" @click.prevent="add" />
        <input type="submit" value="查询" @click.prevent="search" />
      </form>
      <table>
        <tr>
          <th>序号</th>
          <th>姓名</th>
          <th>分数</th>
          <th>时间</th>
          <th>操作</th>
        </tr>
        <tr v-for="(person,index) in persons" :key="person.num">
          <td>
            <input type="text" v-model="person.num" :disabled="isDisabled" />
          </td>
          <td>
            <input type="text" v-model="person.name" :disabled="isDisabled" />
          </td>
          <td>
            <input type="text" v-model="person.score" :disabled="isDisabled" />
          </td>
          <td>
            <input type="text" :value="person.time | dateFormat" disabled />
          </td>
          <td>
            <a href="#" @click.prevent="edit">编辑</a>
            <a href="#" @click.prevent="del(index)">删除</a>
            <a href="#" @click.prevent="more">更多操作</a>
          </td>
        </tr>
      </table>
    </div>
    <script>
      Vue.filter("dateFormat", function (value) {
        let time = new Date(value);
        let Year = time.getFullYear() + "";
        let Month = time.getMonth() + 1 + "";
        let Day = time.getDate() + "";
        return `${Year}年${Month}月${Day}日`;
      });
      var vm = new Vue({
        el: "#example",
        data: {
          isShow: false,
          isDisabled: true,
          obj: {
            font: true,
            bkcolor: false,
          },
          persons: [
            {
              num: 1,
              name: "张三",
              score: 100,
              time: Date.now(),
            },
            {
              num: 2,
              name: "李四",
              score: 90,
              time: Date.now(),
            },
            {
              num: 3,
              name: "王五",
              score: 70,
              time: Date.now(),
            },
          ],
          person: {
            num: "",
            name: "",
            score: "",
            // time:""
          },
        },
        computed: {},
        methods: {
          edit() {
            this.isDisabled = !this.isDisabled;
          },
          del(index) {
            this.persons.splice(index, 1);
          },
          more: function () {
            this.isShow = !this.isShow;
          },
          add() {
            this.person.time = Date.now();
            this.persons.push(this.person);
            this.person = {
              num: "",
              name: "",
              score: "",
            };
          },
          search() {
            let search_Res_Persons = this.persons.filter((person) => {
              if (this.person.score === person.score + "") {
                //此处this.person.score为查询时输入的要查询的分数值，而person.score为数组中各对象的被查询的值
                return true;
              }
            });
            this.persons = search_Res_Persons;
          },
        },
      });
    </script>
  </body>
</html>
```
效果如图：
![图片](https://uploader.shimo.im/f/nPgyb95Ry6qCkUXT.png!thumbnail?fileGuid=jryw69tDDYCvRQtT)



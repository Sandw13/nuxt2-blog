<template>
  <div class="record-list">
    <ul>
      <li v-for="year in years" :key="year.year">
        <h2>{{ year.year }}</h2>
        <div class="contain flex">
          <div class="item flexc" v-for="item in year.items" :key="item.id">
            <b :title="$options.filters.formattime(item.time)">{{ getTime(item.time) }}</b>
            <NuxtLink :class="{multiple: item.images.length>1}" :to="'/records/'+item.id">
              <lazy-img alt="cover" :src="item.images[0].src"/>
            </NuxtLink>
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import {recordList} from '~/utils/data'
import dayjs from "~/utils/_dayjs";

export default {
  name: "index",
  data() {
    return {
      recordList
    }
  },
  asyncData() {
    const years = [];
    recordList.forEach(item => {
      const year = dayjs.utc(item.time).year();
      let exist = years.find(v => v.year === year);
      if (!exist) {
        years.push({
          year,
          items: [item]
        })
      } else {
        exist.items.push(item);
      }
    })
    return {
      years,
    }
  },
  computed: {},
  methods: {
    getTime(stamp) {
      return dayjs.utc(stamp).format('MM.DD');
    }
  }
}
</script>

<style lang="scss">
@import "assets/style/var";
$space: 16px;
.record-list{
  margin: 20px 30px 0 40px;
  padding-bottom: 60px;
  min-width: 800px;
  ul{
    margin: auto;
    li{
      h2{
        color: #1187ab;
        text-shadow: 0 0 2px cyan;
      }
      &:hover{
        .contain{
          border-color: gray;
        }
      }
      .contain{
        margin: $space*2 0;
        .item{
          position: relative;
          &:before{
            position: absolute;
            content: '';
            width: calc(100% + #{$space*3.6});
            height: 1px;
            background: #e9e9e9;
            left: -18px;
            top: 0;
          }
          &:not(:last-of-type){
            margin-right: $space*3.6;
            &:after{
              position: absolute;
              content: '';
              transform: translateY(-50%);
              top: 0;
              right: -$space * 1.8;
              background: rgb(207, 207, 207);
              height: $space*0.8;
              width: 1px;
            }
          }
          &:hover{
            b{
              color: #1187ab;
              text-shadow: 0 0 2px cyan;
            }
          }
          b{
            transform: translateY(-50%);
            padding: 0 5px;
            font-size: 16px;
            background: $background;
            transition: $common-transition;
          }
          a{
            margin-top: 5px;
            position: relative;
            width: $space*10;
            height: $space*10;
            transition: $common-transition;
            .--lazy-img{
              z-index: 3;
              span {
                border: 1px solid #dedede;
                box-sizing: border-box;
              }
              img{
                width: 100%;
                height: 100%;
                object-fit: cover;
              }
            }
            &:hover{
              .--lazy-img{
                transform: translate(-2px, -2px);
              }
            }
            &:before, &:after, .--lazy-img{
              position: absolute;
              top: 0;
              left: 0;
              width: 100%;
              height: 100%;
              transition: $common-transition;
            }
            &.multiple{
              &:before, &:after{
                content: '';
              }
              &:before{
                z-index: 2;
                transform: translate(4px, 4px);
                background: #b8b8b8;
              }
              &:after{
                z-index: 1;
                transform: translate(8px, 8px);
                background: #d8d8d8;
              }
              &:hover{
                &:after{
                  transform: translate(10px, 10px);
                }
              }
            }
          }
        }
      }
    }
  }
}
@include mobile {
  .record-list {
    margin: 20px 10px;
  }
}
</style>

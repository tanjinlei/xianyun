<template>
  <div style="padding:50px;">
    <div class="btns">
      <span v-for="(item, index) in ['第一人称','第二','第三']" :key="index"
      @click="handleclick(index)"
      :class="{ active: index === current }">
          {{item}}
      </span>
    </div>


    <div v-if="current === 0">
        <p>你你你你你你你你你</p>
        <p>你你你你你你你你你</p>
        <p>你你你你你你你你你</p>
        <p>你你你你你你你你你</p>
        <p>你你你你你你你你你</p>
    </div>

    <div v-if="current === 1">
        <p>我我我我我我我我我</p>
        <p>我我我我我我我我我</p>
        <p>我我我我我我我我我</p>
        <p>我我我我我我我我我</p>
        <p>我我我我我我我我我</p>
    </div>

    <div v-if="current === 2">
        <p>他他他他他他他他他</p>
        <p>他他他他他他他他他</p>
        <p>他他他他他他他他他</p>
        <p>他他他他他他他他他</p>
        <p>他他他他他他他他他</p>
    </div>


  </div>
</template>

<script>
export default {
    data () {
        return {
            current: 0
        }
    },

    methods: {
        // 点击的时候改变current：id?
        handleclick(index){
            this.current = index;
        }
    }
};
</script>

<style scoped lang="less">
.btns{
    margin-bottom: 20px;
    span{
        cursor:pointer;
        border: 1px #ddd solid;
        padding: 10px;
    }

    .active{
        color: red;
        border-color: red;

    }
}

</style>
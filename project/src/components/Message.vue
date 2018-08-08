<template>
  <div class="message">
    <ul>
        <span class="time">{{`${this.checkTime(new Date(item.create_time_stamp).getHours())}
            :${this.checkTime(new Date(item.create_time_stamp).getMinutes())}`}}</span>
        <p class="new" v-if="index === 0 ? true : false">new</p>
        <li class="list">
            <p class="person">{{item.speaker.name}}</p>
            <p class="content">{{item.content}}</p>
        </li>
    </ul>
  </div>
</template>


<script>
export default {
  name: 'Message',
  props: ['item', 'length', 'index'],
  methods: {
    checkTime(i) {
      let j = i;
      if (i < 10) j = `0${i}`;
      return j;
    },
  },
};
</script>

<style scoped>
.list{
    margin-left: 60px;
    padding-left: 5px;
}
ul{
    list-style-type: circle;
    font-size: 12px;
    margin-right: 10px;
    padding-left: 10px;
}
.time{
    float: left;
    font-weight: bold;
}
.person{
    color:gray;
}
.new{
    color:red;
    float: left;
    clear:both;
    margin-left: 10px;
}
</style>

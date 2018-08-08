<template>
  <div class="room">
    <h3>房间号:{{ roomId }}</h3>
    <div class="content">
      <Message v-for="(item, index) in items" v-bind:key="item.id" v-bind:item="item"
      v-bind:length="items.length" v-bind:index="index"></Message>
    </div>
    <p v-if="!this.update && !this.over">刷新中...</p>
    <p v-if="this.over">已经没有内容了</p>
  </div>
</template>

<script>
import Message from './Message';

export default {
  name: 'Room',
  data() {
    return {
      items: [],
      update: true,
      streamId: '',
      oldestSid: '',
      newestSid: '',
      roomId: 126727,
      over: false,
      startY: 0,
    };
  },
  components: {
    Message,
  },
  methods: {
    getData(url = '') {
      return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest();
        xhr.open('get', url, true);
        xhr.send();
        xhr.onreadystatechange = () => {
          if (xhr.readyState === 4) {
            if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
              resolve(JSON.parse(xhr.responseText));
            } else {
              reject(new Error(`失败：${xhr.status} ${xhr.statusText}`));
            }
          }
        };
      });
    },
    pushItems(response) {
      response.data.forEach((element) => {
        this.items.push(element);
      });
      this.oldestSid = this.items[this.items.length - 1].sid;
      this.newestSid = this.items[0].sid;
    },
    insertItems(response) {
      response.data.forEach((element) => {
        this.items.unshift(element);
      });
      this.newestSid = this.items[0].sid;
    },
    async getNewMessage() {
      const response = await this.getData(`/api/stream/${this.streamId}/message/?indexflag=1&room_id=${this.roomId}&sid=${this.newestSid}`);
      if (response.data.length !== 0) {
        this.insertItems(response);
      }
      if (response.live_status === 3) {
        console.log('直播结束');
        return true;
      }
      console.log('获取直播');
      setTimeout(() => this.getNewMessage(), 1000);
      return true;
    },
    async getMessage() {
      const response = await this.getData(`/api/room/?room_ids=${this.roomId}`);
      this.streamId = response.data[0].streams[0];
      const responseData = await this.getData(`/api/stream/${this.streamId}/message/?room_id=${this.roomId}`);
      this.pushItems(responseData);
      return true;
    },
    async updateMessage() {
      if (!this.over) {
        const response = await this.getData(`/api/stream/${this.streamId}/message/?indexflag=0&room_id=${this.roomId}&sid=${this.oldestSid}`);
        if (response.data.length === 0) {
          this.over = true;
          return true;
        }
        this.pushItems(response);
        console.log('已刷新');
      }
      return true;
    },
  },

  created() {
    this.getMessage().then(() => {
      this.getNewMessage();
    }).catch(() => {
      console.log('网络错误');
    });
  },

  mounted() {
    document.addEventListener('touchstart', (e) => {
      e.preventDefault();
      this.startY = e.touches[0].pageX;
    }, false);
    document.addEventListener('touchmove', (e) => {
      const scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
      const clientHeight = document.documentElement.clientHeight || document.body.clientHeight;
      const scrollHeight = document.documentElement.scrollHeight || document.body.scrollHeight;
      const moveY = e.changedTouches[0].pageY - this.startY;
      if (scrollTop + clientHeight === scrollHeight && moveY > 0) {
        if (this.update) {
          this.update = false;
          setTimeout(() => {
            this.updateMessage().catch(() => {
              console.log('刷新错误');
            }).finally(() => { this.update = true; });
          }, 500);
        }
      }
    }, false);
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.room{
  border: 1px solid #ddd;
  width: 95%;
  margin: 0 auto;
}

h3{
  width:100%;
  font-size: 15px;
  margin-left: 5px;
}
p{
  text-align: center;
  color: red;
}
</style>

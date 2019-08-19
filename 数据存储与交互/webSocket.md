# webSocket

```vue
<template>
  <div class="app-container">
  </div>
</template>

<script>
export default {
  data() {
    return {
      websock: null,
      dataList: []
    }
  },
  created() {
    //页面刚进入时开启长连接
    this.initWebSocket()
  },
  destroyed: function() {
    //页面销毁时关闭长连接
    this.websocketclose()
  },
  methods: {
    initWebSocket() {
      //初始化weosocket
      const wsuri = `ws://Col0ring/frames` //ws地址
	  //设置token!!!关键点!!!
      this.websock = new WebSocket(wsuri, ['Bearer', this.$store.getters.token])

      this.websock.onopen = this.websocketonopen.bind(this)
      this.websock.onerror = this.websocketonerror.bind(this)
      this.websock.onmessage = this.websocketonmessage.bind(this)
      this.websock.onclose = this.websocketclose.bind(this)
    },

    websocketonopen() {
      console.log('connect')
    },
    websocketonerror(e) {
      //错误
      console.log('connect error')
    },
    websocketonmessage(e) {
      //数据接收

      const redata = JSON.parse(e.data)
      this.dataList.push(redata)
    },
    websocketclose(e) {
      //关闭
      console.log('connection closed')
    }
  }
}
</script>
```

**封装了一个方法**

```js
import store from '@/store'

export default {
  // 保证整个项目只有一个socket实例
  ws: null,
  init(url, onMessage, onError) {
    if (!this.ws) {
      // 实例化
      this.ws = new WebSocket(`ws://Col0ring/frames`, [
        'Bearer',
        store.getters.token
      ])
    }
    this.ws.onopen = event => {
      console.log('connect')
      console.log(this.ws)
    }
    this.ws.onmessage = event => {
      const message = JSON.parse(event.data)
      onMessage && onMessage(message)
    }

    this.ws.onerror = error => {
      this.ws.close()
      onError && onError(error)
    }

    this.ws.onclose = () => {
      console.log('connection closed')
      this.ws.close()
      this.ws = null
    }
  },
  send(msg) {
    this.ws.send(JSON.stringify(msg))
  },
  close() {
    this.ws.close()
  }
}
```


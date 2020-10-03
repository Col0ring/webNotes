## 前言

最近升级了下 vue-cli，发现已经能够创建 Vue 3  的项目了，为了跟进最近技术，决定开发一个`swipe-cell`组件来学习一下如何使用 Vue 3。

## 创建项目

目前官方有两种脚手架供我们生成项目，分别是原来的 vue-cli 和 尤大新开发的 vite。我这边是使用 vue-cli 进行创建的，vite 等后面有时间再来研究一下。

将 vue-cli 升级到最新版本，然后使用`vue create 项目名`创建一个 Vue 3 的项目。
![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5b2bcf37bdb24c098e83cabd16050c11~tplv-k3u1fbpfcp-zoom-1.image)
![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7399ddaad3d94a6698c5ae7b9df2e696~tplv-k3u1fbpfcp-zoom-1.image)

然后选择使用 3.x 版本就行了。

## 组件编写
### 需求分析
`swipe-cell`组件主要是对于`touch`相关事件的相关操作进行监听来实现滑动的，所以基本操作只需要在内部定义响应的事件进行滚动监听就好。然后用户再点击到非当前滑动区块时将之前已经滑动的区块重置，并且点击其他模块进行响应的操作可以单独提出进行封装。

### 代码实现
> 以下代码都是使用 typescript 编写

下面是 `setup()`内的基本实现逻辑：
```ts
const state = reactive({
  start: 0,
  current: 0,
  opening: false,
  swiping: false
})

// 右侧的隐藏按钮
const actionBtns = ref<HTMLDivElement | null>(null)

// 移动距离
const translateStyle = computed(() => {
  return {
    transform: `translateX(${state.current}px)`
  }
})
// 按钮的宽度
const actionsWidth = computed(() => {
  return Math.ceil(actionBtns.value?.getBoundingClientRect().width || 0)
})
// 监听 swiping 和 opening 的变化
watchEffect(() => {
  // 如果 state.opening 放前面 touchEnd 结束将不会触发 else 分支
  if (!state.swiping && state.opening) {

    state.current = -actionsWidth.value
  } else {
    state.current = 0
  }
})

const touchStart = (e: TouchEvent) => {
 // 开始滑动，记录位置
  state.start = e.changedTouches[0].clientX
  // 如果已经打开，直接关闭
  if (state.opening) {
    state.opening = false
  }
}
const touchMove = (e: TouchEvent) => {
  const current = e.targetTouches[0].clientX
  const scale = current - state.start
  // 组件只进行左滑，所以往右不进行操作
  if (scale > 0 || state.opening) {
    return
  }
  const swipeScale =
        Math.abs(scale) < actionsWidth.value ? scale : -actionsWidth.value
  state.swiping = true
  state.current = swipeScale
}
const touchEnd = () => {
  // 如果滑动距离大于一半就直接全部打开
  if (Math.abs(state.current) >= actionsWidth.value / 2) {
    state.opening = true
  }
  state.swiping = false
}
// 默认有个删除按钮，所以可绑定 delete 事件
const handleDelete = (e: MouseEvent) => {
  emit('delete', e)
}
return {
  actionBtns,
  translateStyle,
  touchStart,
  touchMove,
  touchEnd.
  handleDelete
}
```
然后就是对点击其他位置时的情况作判断，这里我们单独提出来封装，下面的代码借鉴了 react 的 [ahooks](https://github.com/alibaba/hooks)：
```ts
// ./utils/useClickAway.ts
import { onMounted, Ref, onUnmounted } from 'vue'

const defaultEvent = 'click'
type EventType = MouseEvent | TouchEvent

const useClickAway = (
  onClickAway: (e: EventType) => void,
  target: Ref<HTMLElement | null>,
  eventName = defaultEvent
) => {
  const handler = (event: Event) => {
    if (!target.value || target.value.contains(event?.target as Node)) {
      return
    }
    onClickAway(event as EventType)
  }

  onMounted(() => {
    document.addEventListener(eventName, handler)
  })
  onUnmounted(() => {
    document.removeEventListener(eventName, handler)
  })
}

export default useClickAway
```
最后再加入到组件中：
```ts
// ...
// 最外层的 swipe-cell，用于判断是否点击到组件外部
const swipeCell = ref<HTMLDivElement | null>(null)

// 如果点击到了其他位置就恢复到原位
useClickAway(
    () => {
      state.opening = false
    },
    swipeCell,
    'touchstart'
)
// ...
return {
  swipeCell
  //...
}
```


### 全部组件代码

```vue
<template>
  <div class="swipe-cell-container" ref="swipeCell">
    <div
      class="cell-wraper"
      :style="translateStyle"
      @touchstart.passive="touchStart"
      @touchmove.passive="touchMove"
      @touchEnd.passive="touchEnd"
    >
      <div class="cell-content">
        <slot>
          <span>{{ content }}</span>
        </slot>
      </div>
      <div class="cell-actions" ref="actionBtns" @touchstart.stop.passive>
        <slot name="actions">
          <button @click="handleDelete" class="delete-btn">
            {{ delText }}
          </button>
        </slot>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, reactive, computed, watchEffect } from 'vue'
import useClickAway from './utils/useClickAway'

interface SwipeCellProps {
  content: string
  delText: string
}

export default defineComponent({
  name: 'SwipeCell',
  props: {
    content: {
      type: String,
      default: ''
    },
    delText: {
      type: String,
      default: '删除'
    }
  },
  setup(props: SwipeCellProps, { emit }) {
    const state = reactive({
      start: 0,
      current: 0,
      opening: false,
      swiping: false
    })

    const swipeCell = ref<HTMLDivElement | null>(null)
    const actionBtns = ref<HTMLDivElement | null>(null)

    const translateStyle = computed(() => {
      return {
        transform: `translateX(${state.current}px)`
      }
    })
    const actionsWidth = computed(() => {
      return Math.ceil(actionBtns.value?.getBoundingClientRect().width || 0)
    })

    watchEffect(() => {
      // 如果 state.opening 放前面 touchEnd 结束将不会触发 else 分支
      if (!state.swiping && state.opening) {
        state.current = -actionsWidth.value
      } else {
        state.current = 0
      }
    })

    useClickAway(
      () => {
        state.opening = false
      },
      swipeCell,
      'touchstart'
    )

    const touchStart = (e: TouchEvent) => {
      state.start = e.changedTouches[0].clientX
      if (state.opening) {
        state.opening = false
      }
    }
    const touchMove = (e: TouchEvent) => {
      const current = e.targetTouches[0].clientX
      const scale = current - state.start
      if (scale > 0 || state.opening) {
        return
      }
      const swipeScale =
        Math.abs(scale) < actionsWidth.value ? scale : -actionsWidth.value
      state.swiping = true
      state.current = swipeScale
    }
    const touchEnd = () => {
      if (Math.abs(state.current) >= actionsWidth.value / 2) {
        state.opening = true
      }
      state.swiping = false
    }
    const handleDelete = (e: MouseEvent) => {
      emit('delete', e)
    }
    return {
      swipeCell,
      actionBtns,
      translateStyle,
      handleDelete,
      touchStart,
      touchMove,
      touchEnd
    }
  }
})
</script>
<style lang="less" scoped>
.swipe-cell-container {
  border-bottom: 1px solid #eee;
  width: 100%;
  overflow: hidden;
  .cell-wraper {
    display: flex;
    position: relative;
    transform: translateX(0);
    transition: transform 0.2s linear;
    width: 100%;
    .cell-content {
      width: 100%;
    }
    .cell-actions {
      display: flex;
      align-items: center;
      justify-content: center;
      height: 100%;
      position: absolute;
      right: 0;
      top: 0;
      transform: translateX(100%);
      .delete-btn {
        height: 100%;
        font-size: 14px;
        color: #eeeeee;
        padding: 0px 24px;
        border: none;
        outline: none;
        background: hsl(348, 100%, 61%);
      }
    }
  }
}
</style>
```

到这里，一个简单的`swipe-cell`组件就完成了:
![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fa2066bbed4f4099b69db2ff17b5c1b4~tplv-k3u1fbpfcp-zoom-1.image)

## 发布成库
编写完组件代码后，我们尝试将其作为 Vue 的插件使用，并发布到 npm 上。首先在组件的文件同级新建`index.ts`文件：
```ts
import { Plugin } from 'vue'
import SwipeCell from './index.vue'

export { SwipeCell }

export default {
  install: (app, options) => {
    app.component(options?.name || 'SwipeCell', SwipeCell)
  }
} as Plugin
```
然后手动编写类型声明文件：
```ts
// ./types/swipe-cell.d.ts
import { Plugin } from 'vue'

export interface SwipeCellProps {
  content: string
  delText: string
}
declare class SwipeCell {
  $props: SwipeCellProps
}
declare const _default: Plugin
export { SwipeCell }

export default _default
```
最后再根据 vue-cli 对配置打包就可以发布成库了，具体的发布细节这里就不再赘述了。

## 结语
本文只使用了部分 Vue3 的 Api，算是对于 Vue3 的一次尝鲜。文中如果有什么不足之处，还请各位大佬及时指出。

全部代码目前已提交到 [github](https://github.com/Col0ring/swipe-cell)。


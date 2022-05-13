---
title: 关于Svelte开发WebComponent的一个踩坑过程
date: 2022/5/13 14:00:00
description: 使用Svelte开发WebComponent中遇到的各种问题
tag: svelte,WebComponent
author: Mowtwo
---

# 关于Svelte开发WebComponent的一个踩坑过程

事情的起因是我的一个朋友跑来问我如何找到一些Web Component写的组件，因为他的一个项目需要用到一个简单的tree组件。

虽然我不清楚现在什么样的项目会专门需要用到web component来进行开发，不过本着看着不算难，所以要试试的原则，我跟他说，可以试试Svelte。

作为一个~~非知名~~Svelte布道者，我个人在最近的很长一段时间里都在尝试并使用了Svelte，虽然大部分都是用到了个人项目中，比如我自己个人主页，还有一个简单的开源项目的前端部分。

> **个人主页**: [https://mowtwo.com](https://mowtwo.com)/[https://github.com/mowtwo/mow-page-public](https://github.com/mowtwo/mow-page-public)
>
> **FFServer**:  [https://github.com/DimCyan/ffserver](https://github.com/DimCyan/ffserver)/[https://github.com/mowtwo/ffserver_frontend](https://github.com/mowtwo/ffserver_frontend)

在之前我就看到一些用Svelte开发WebComponent的文章，所以在我提出这个建议的时候，我以为事情会变得顺其自然。

不过对方很快给出了一些问题，首先他不是一个专业的前端，Svelte虽然简单，但是目前他还不会。而且其实这次需求下，仅仅只是需要一个tree组件，能够让后端开箱即用。

那要解决问题，就只能考虑另一个办法，那就是现成的组件库。Svelte的冷门程度确实还是超乎了我的想象，虽然常说在国外，Svelte还是有一定的用户的，但是实际上找寻下来，Svelte并没有类似于Vue跟React那种特别通用的组件库。找到的几个大都也是基于Material Design的，组件数量实在很少不说，而且都没有tree组件。

虽然最终我找到了一个基于TailwindCSS，MD设计风格，并且有tree组件。但是这并没有解决我的问题，因为我发现，这玩意样式压根不生效，折腾了半小时我放弃了，可能是我的使用姿势不对吧。

但是作为一个喜欢折腾的人，我不能让我好不容易推荐出去的Svelte成为一个笑话。我打算自己写，对，参考上面找到的那个框架的文档里演示的tree组件，写一个，因为功能看起来并不复杂。

## 踩坑的开始

编写tree的过程确实不复杂，我也模仿上面的那个框架，选择引入TailwindCSS来快速解决样式问题。

编写出来的代码很简单，实现了一个tree-item组件，然后又封装了一层tree-view来负责实现递归封装，这个组件最终只需要接收一个tree的props，就可以完成一个tree的生成。

下面是tree-item组件的代码，不过是复制的最终完成版本的代码还原的

```vue
<script context="module" lang="ts">
  export type TreeItemEvents = {
    toggleExpand: boolean;
  };
</script>

<script lang="ts">
  import { createEventDispatcher } from "svelte";

  import { slide } from "svelte/transition";

  export let leaf = false;
  export let text = "";
  export let expand = false;
  export let selected = false;

  const dispatch = createEventDispatcher<TreeItemEvents>();
</script>

<div class="item">
  {#if leaf}
    <div
      class="cursor-pointer text-gray-600 flex items-center h-40px bg-white transition hover-bg-eee"
      style={selected ? "background-color: rgba(119, 197, 250, 0.315)" : ""}
      on:click
    >
      <div class="ml-4"><slot /></div>
    </div>
  {:else}
    <div
      class="cursor-pointer text-gray-600 flex items-center h-40px bg-white transition hover-bg-eee"
      on:click={() => {
        expand = !expand;
        dispatch("toggleExpand", expand);
      }}
    >
      <div
        class="ml-1 text-12px text-gray-600 flex item-center"
        style="height: 100%; width:16px;"
      >
        <div class="flex items-center transition" class:rotate-90={expand}>
          <svg
            class="icon"
            viewBox="0 0 1024 1024"
            version="1.1"
            xmlns="http://www.w3.org/2000/svg"
            xmlns:xlink="http://www.w3.org/1999/xlink"
            width="16"
            height="16"
            ><path
              d="M593.450667 512.128L360.064 278.613333l45.290667-45.226666 278.613333 278.762666L405.333333 790.613333l-45.226666-45.269333z"
              p-id="1089"
            /></svg
          >
        </div>
      </div>
      <div class="ml-1">{text}</div>
    </div>
    {#if expand}
      <div transition:slide class="ml-1">
        <slot />
      </div>
    {/if}
  {/if}
</div>

```



在网页上，用普通编译模式预览后，我就开始准备编译到WebComponent。

### 开启WebComponent编译模式

在Svelte中，打包WebComponent无非做两件事，一个就是在打包工具的Svelte插件里开启customElement的编译选项，一个就是给每一个组件都添加一个自定义标签名。

由于我用的是vite创建的项目，所以就是在vite的Svelte插件里设置

```ts
import { defineConfig } from 'vite'
import { svelte } from '@sveltejs/vite-plugin-svelte'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [svelte({
    compilerOptions: {
      customElement: true,
    },
  })],
})

```

然后就是给各个组件添加自定义标签名，这个是通过Svelte提供的内置元素实现的

```vue
<svelte:options tag="tree-item" />
```

==注意：每一个用到的svelte文件都得加==，一开始我以为只需要给导出的那个添加，但是其实是每一个元素都得加

而且还要注意另外一点，svelte组件被注册成WebComponent后，在组件之间互相引用也必须采用WebComponent的方式进入引入，比如我在用tree-view组件包装tree-item的时候，原来的写法是

```vue
<script lang="ts">
    import TreeItem from "./TreeItem.svelte";
</script>
<TreeItem
      text={item.text}
      on:toggleExpand={(e) => {
        dispatch("itemToggle", {
          target: item,
          expand: e.detail,
        });
      }}
    >
      <svelte:self
        tree={item.children}
        on:leafClick={(e) => {
          handleChildrenLeafClick(e);
        }}
        on:itemToggle={(e) => {
          dispatch("itemToggle", e.detail);
        }}
      />
    </TreeItem>
```

但是注册后，则改成下面的写法

```vue
<svelte:options tag="tree-view" />
<script lang="ts">
    import TreeItem from "./TreeItem.svelte";
</script>
<tree-item
      text={item.text}
      on:toggleExpand={(e) => {
        dispatch("itemToggle", {
          target: item,
          expand: e.detail,
        });
      }}
    >
      <svelte:self
        tree={item.children}
        on:leafClick={(e) => {
          handleChildrenLeafClick(e);
        }}
        on:itemToggle={(e) => {
          dispatch("itemToggle", e.detail);
        }}
      />
    </tree-item>
```

编译并没有遇到太大的问题，不过预览的方式需要进行修改，Svelte默认编译模式下，通过引入App组件然后new App后挂载target到页面中的一个元素下。

而现在则不再需要这些步骤，只需要把需要用到的组件引入，然后在HTML里直接编写WebComponent元素使用就可以。

因此需要继续修改

```ts
import "TreeItem.svelte";
import "TreeView.svelte";
```

```html
<body>
    <tree-view></tree-view>
</body>
```

运行dev，在浏览器中没有报错，并且用devtool进行查看，可以发现shadow dom成功渲染。

看起来还算顺利，但是当我们真正开始使用后，新的问题接踵而来。

### 传递参数问题

我们在上面使用WebComponent时，还没有传递任何的props，但是实际上我们的tree-view组件是需要接受一个叫做tree的对象数组来渲染内容的，因此我们尝试传递

```html
<body>
    <tree-view tree="[{text:''}]"></tree-view>
</body>
```

看起来没啥问题，但是实际上我们会得到一个报错。

其实报错的内容完全不需要在意，因为看了也没啥用，问题很明显，那就是通过HTML的attribute直接传递的props，会被当成字符串直接传递。

而且组件内，实际上我们是回去遍历tree数组，然后还要做判断，类似这样

```vue
{#each tree as item}
  {#if !item.children}
    <tree-leaf
      selected={item.selected}>{item.text}</tree-leaf
    >
  {:else}
    <tree-item
      text={item.text}>
      <svelte:self
        tree={item.children}
      />
    </tree-item>
  {/if}
{/each}
```

很明显，字符串是没办法实现这个情况的。

在解决问题之前，我们要分析一下这里涉及到两种情况。在我们使用普通的HTML标签的时候，设置标签的属性一般有两种方式，一种就是在HTML里之前给标签添加属性，还有一种就是通过js去直接设置属性或者使用setAttribute。其实setAttribute的情况就跟直接在HTML添加属性是类似的行为，因为setAttribute传递的参数值必须是字符串。

那这里就要单独分析一下直接设置的情况，这里先说一下结论，那就是直接通过js属性的方式设置的值是可以保留类型的。

这个是我在原生网页与Vue内测试后得出的结果，下面给出Vue的案例

```vue
<script setup lang="ts">
import { onMounted, ref } from "vue";

const tree = ref([
  {
    text: "Hello",
    children: [
      {
        text: "World",
        children: [
          {
            text: "!",
          },
        ],
      },
    ],
  },
]);

const treeRef = ref<{ $on: Function }>();
onMounted(() => {
  treeRef.value?.$on("leafClick", function (e:CustomEvent) {
    console.log("leafClick",e.detail); // 这里内部自定义事件传递出来的原始对象的Proxy对象，所以保留了Vue的reactive特点，直接修改后可以触发Vue的更新
    e.detail.text = '???'
  });
});
</script>

<template>
  <tree-view :tree="tree" ref="treeRef" />
</template>
```

不过为了能够让WebComponent使用起来更像普通的HTML标签，我们也得兼容一下这种情况，因此我们创建一个新的renderTree来作为最终渲染的数组，而tree改成可以接受数组跟字符串的形式，在组件更新时做一个自动转换，这里其实也可以写成computed的形式，不过我为了方便写判断，所以写成了代码块。

```typescript
  let renderTree: Tree[] = [];

  $: {
    if (Array.isArray(tree)) {
      renderTree = tree;
    } else if (typeof tree === "string") {
      renderTree = JSON.parse(tree);
    } else {
      console.warn("tree必须是合法的JSON字符串或对象数组");
    }
  }
```



### 样式不生效

解决了各种渲染问题，最终也终于看到页面中出现了内容，但是一个很基础，但是被我忘记的东西出现了。

我之前用了TailwindCSS，样式全部无效了。这里其实简单想想就知道了，WebComponent为了防止样式污染，所以对于整个WebComponent进行了密封，内部的样式无法影响到内部，外部的样式也无法影响到内部，而TailwindCSS的样式是会被编译进单独的CSS文件的。

因此只能将样式重新在模板文件的style里写了一次，不过好歹很有效，样式出现了，虽然跟TailwindCSS写的相比缺少了一些兼容性。

修改后终于可以看到简单的效果，基本符合最初的设计

![image-20220513134751653](https://raw.githubusercontent.com/mowtwo/pic-go/main/markdown/image-20220513134751653.png)

### 事件不生效

本来以为事情已经完美解决，但是很快另一个问题就一下又让我浪费了两个小时。

原本以为Svelte打出来的WebComponent基本上符合标准的HTML标签的用法，因为上面的props传递的方式迷惑了我，因此我很自信的认为绑定事件应该也是一样的。

我在测试里写下了

```js
const tree  = document.querySelector('tree-view')
tree.addEventListener('leafClick',function(e) {
    console.log("leafClick",e.detail)
})
tree.addEventListener('itemToggle',function(e) {
    console.log("itemToggle",e.detail)
})
```

结果很快被打脸，压根不生效，一直到这里，我原本以为半小时就能搞定的小问题，其实已经浪费了我一下午将近五个小时的时间。将近崩溃的我只能开始求助于Google（不是没百度，而是百度压根找不到）。

这里其实也是分为两部分解决，并且是在同一个GitHub issue里看到解决方案：

[Events are not emitted from components compiled to a custom element · Issue #3119 · sveltejs/svelte (github.com)](https://github.com/sveltejs/svelte/issues/3119)

首先就是关于事件的绑定的问题，那就是不能直接使用原生的事件监听系统，Svelte在编译组件的时候会在组件对象看挂载一个自定义的监听器`$on`。

将监听修改

```js
const tree  = document.querySelector('tree-view')
tree.$on('leafClick',function(e) {
    console.log("leafClick",e.detail)
})
tree.$on('itemToggle',function(e) {
    console.log("itemToggle",e.detail)
})
```

这样修改后，事件成功触发，但是我很快发现了另外一个问题，就是只有leafClick被触发，而自定义的折叠/展开的触发事件itemToggle无法触发。

这里就不说排查过程，实际解法就是，Svelte的createEventDispatcher创建的dispatch函数是无法将事件传递出WebComponent，原因暂时未知，没有去研究。而其中一个事件会触发的原因是leafClick是直接映射的原生的click事件，这里其实挺奇怪的。

根据issue里解法，就是在触发普通dispatch的时候还需要去调用原生DOM的dispatchEvent，这里要注意在template是没办法直接拿到组件顶层的DOM对象，不过Svelte提供了`get_current_component`来获取，所以解决并不复杂，将有使用到dispatch的地方进行简单的改造

```ts
const thisComponent = get_current_component();

  const svelteDispatch = createEventDispatcher<TreeItemEvents>();

  const dispatch: typeof svelteDispatch = (type, detail) => {
    thisComponent?.dispatchEvent?.(
      new CustomEvent(type, {
        detail,
      })
    );
    return svelteDispatch(type, detail);
  };
```

至此问题全部解决，终于结束了。

## 总结

其实这个开发过程总体都是因为不熟悉各种WebComponent相关的特性造成的问题，所以踩坑在所难免。

不过最终虽然解决了问题，不过个人感觉项目已经基本上算是完蛋了。不过最终还是在朋友[@alexzhang1030 (github.com)](https://github.com/alexzhang1030)的帮助下帮忙配置了发包，目前也将开发的包暂时发布到了npm上。

## 相关链接

- [alexzhang1030 的个人主页 - 动态 - 掘金 (juejin.cn)](https://juejin.cn/user/616175907901309)
- [mowtwo/svelte-tree: 使用svelte构建一个web component的tree组件 (github.com)](https://github.com/mowtwo/svelte-tree)
- [@mowtwo/svelte-tree - npm (npmjs.com)](https://www.npmjs.com/package/@mowtwo/svelte-tree)
- [Events are not emitted from components compiled to a custom element · Issue #3119 · sveltejs/svelte (github.com)](https://github.com/sveltejs/svelte/issues/3119)
---
layout: post
title: Learning Notes - from Weex——关于移动端动态性的思考、实现和未来
---

## 移动端的动态性定义
所谓移动端的动态性，就是把移动应用本身的**灵活性**、迭代更新的**周期**和**成本优化**到极致。 

一个移动应用，客观上需要更多的尝试和探索，甚至是“试错”，而这种动态化的程度和产品迭代演进的速度有着强烈的正相关，我们不必要为这些动态性在多个端投入重复的精力，更不应该因此而频繁的触发新版本。所以动态性问题在今天的移动端显得尤其重要。

### 移动台满足动态性的一些方案
- Hybrid方案：以WebView为容器，以HTML5为基石，通过定义native特性的扩展来支持的动态化产品研发。 这类方案通常具有非常高的动态性，但存在的问题和动态性本身一样明显，那就是性能和展现效果上的不足，而且想把其优势在工程中充分发挥出来，对开发者在前端知识和经验上的积累也有较高的要求
- 结构化native view方案：以native view为容器进行 native 级别的渲染，并定义一套描述视图结构的数据格式 (如 XML 或 JSON 等) ，然后通过动态改变或请求新的这样的数据信息达到动态化的界面效果，比如阿里巴巴集团内出现 (过) 的 WeApp、鸟巢、Dynative、PageKit 等，这类方案依赖一个结构化的界面描述，并重点保障纯展现输出维度的动态性，各有千秋，但有一些共性的不足之处，比如对其它维度的动态性处理，比如逻辑的动态性，加载策略的动态性等。
- React Native方案：大家习惯简称其为RN，以native为渲染引擎，通过脚本引擎支持界面Virtual DOM的转换和逻辑控制，来实现界面的动态性。RN效果并不令人满意，首先是RN量级非常重，**在请求、加载、渲染、交互、稳定性等层面都不够理想**，而整个技术方案在社区的迭代和**演进过程也一直充满着不确定性**，这给团队产品级别的运用和后期跟进带来了很大的困惑。

### Weex方案
Weex 核心设计理念是三端一体化的动态化解决方案，云端同学实现实时发布和动态部署、模版预解析处理，前端同学在 JS Framework 实现动态内容解析并处理成 Virtual DOM，客户端同学提供渲染实现和 native 特性的支持，接下来业务同学根据 DSL 实现动态内容的开发或配置即可。

#### How Weex works
eex is a extendable cross-platform solution for **dynamic programming and publishing projects**. In the source code you can write pages or components with **<template>**, **<style>** and **<script>** tags, and then ***transform*** them into **bundles** for deploying. In server-side we can use these JS bundles for client request. When client get a bundle from server, it will **be processed by client-side JavaScript engine and manages the native view rendering**, the native API invoking and user interactions.

Whole Workflow

Weex file --------------frontend(source code)
↓ (transform) --------- frontend(build tool)
JS bundle ------------- frontend(bundle code)
↓ (deploy) ------------ server
JS bundle in server --- server
↓ (compile) ----------- client(js-engine)
Virtual DOM tree ------ client(weex-jsframework)
↓ (render) ------------ client(render-engine)
Native view ----------- client(render-engine)
According to the workflow above, you need:

- Transformer: A **nodejs tool** to transform the source code into the bundle code.
- JS Framework: A **JavaScript framework** runing in the client which manage Weex instance. The instance which created from a JS bundle builds virtual DOM tree. Also it sends/receives native calls for native view rendering, native APIs and user interactions.
- Native Engine: There are many different ports for different platforms: iOS/Android/HTML5. They have the same components design, module APIs design and rendering effect. So they can work with the one and the same JS Framework and JS bundles. (cordova alike)



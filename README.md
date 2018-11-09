# mobile-slide

> 在vue.js中使用原生js实现移动端无缝轮播图组件

* 背景: 用vue.js来做数据绑定,能加快前端的开发效率,然而很多情况下,需要我们手动操作DOM,比如说写一个轮播图;很多人为了加快开发的效率(偷懒),直接用一些轮播图的插件,比如:swiper.js或者vue-easy-slide这些;但是这些插件一般都很臃肿,而且还需要阅读他的api,然后还要根据实际需求修改他的样式,wtf,忍不了了;于是我决定自己写一个移动端的轮播图组件,以后要用到直接调用就好了~

* 对比pc端的轮播图(对于很多前端来说,简直小菜一碟),移动端的轮播图要复杂一些,主要复杂在他需要三个touch事件,以及要如何实现无缝轮播,;

* 试了很多种方法,终于摸索出了一个自己比较满意的实现,大概200行代码,现在记录一下;

* 有需要的小伙伴可以克隆这个仓库,你可以用自己的样式覆盖我写的样式,其他地方根据实际情况修改即可.

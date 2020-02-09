comment.js
===

This project is archived and will no more be mantained! Please use [gitalk](https://github.com/gitalk/gitalk) or [gitment](https://github.com/imsun/gitment) instead.

[![GitHub release](https://img.shields.io/github/release/wzpan/comment.js.svg)](https://github.com/wzpan/comment.js/releases)
[![license](https://img.shields.io/github/license/wzpan/comment.js.svg)](https://github.com/wzpan/comment.js/blob/master/LICENSE)

<!-- markdown-toc start - Don't edit this section. Run M-x markdown-toc-generate-toc again -->
**Table of Contents**

- [comment.js](#commentjs)
    - [Demo](#demo)
    - [Dependencies](#dependencies)
    - [How to use](#how-to-use)
        - [0. Add stylesheets and JavaScript files](#0-add-stylesheets-and-javascript-files)
        - [1. Create Github Repo](#1-create-github-repo)
        - [2. Register an OAuth App](#2-register-an-oauth-app)
        - [3. Comment Thread](#3-comment-thread)
        - [4. Recent Comment List](#4-recent-comment-list)
    - [Browser support](#browser-support)
    - [How to contribute](#how-to-contribute)

<!-- markdown-toc end -->

Pure JavaScript to make a Disqus-like comment system. Use issue system of code hosting sites like Github/OSChina as the backend. Easily to use. Free for running.

* [中文文档](http://www.hahack.com/codes/comment-js/#项目介绍)

## Demo

* [Demo Comment Thread](http://hahack.com/comment.js#comment-thread)
* [Demo Recent Comment List](http://hahack.com/comment.js#recent-comments)

## Dependencies

* [jQuery](https://jquery.com/) - for making Ajax requests and manipulating DOM.
* [marked](https://github.com/chjj/marked) - for rendering markdown.
* [timeago.js](https://github.com/hustcc/timeago.js) - for formatting time labels.
* [highlight.js](http://highlightjs.org) - for syntax highlighting(optional).
* [spin](https://github.com/fgnass/spin.js) - for displaying a spinning indicator(optional).

## How to use

> For people using Hexo as static blog generator, they can try a theme [hexo-theme-freemind](http://github.com/wzpan/hexo-theme-freemind) with comment.js as the default comment system that works out of box.

### 0. Add stylesheets and JavaScript files

``` html
<!-- stylesheet -->
<link rel="stylesheet" href="path_to_comment_css/comment.css">

<!-- for IE support -->
<!--[if lte IE 9]>
	<script src="https://cdnjs.cloudflare.com/ajax/libs/es5-shim/4.5.9/es5-shim.min.js"></script>
	<script src="https://cdnjs.cloudflare.com/ajax/libs/es5-shim/4.5.7/es5-sham.min.js"></script>
<![endif]-->

<!-- javascripts -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/2.0.3/jquery.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/marked/0.3.6/marked.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/timeago.js/3.0.2/timeago.min.js"></script>

<!-- loading spin indicator(optional) -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/spin.js/2.3.2/spin.min.js"></script>

<!-- syntax highlighting(optional) -->
<link rel="stylesheet" href="//cdnjs.cloudflare.com/ajax/libs/highlight.js/9.12.0/styles/default.min.css">
<script src="//cdnjs.cloudflare.com/ajax/libs/highlight.js/9.12.0/highlight.min.js"></script>
<script type="text/javascript">
  marked.setOptions({
  highlight: function (code, lang) {
     return hljs.highlightAuto(code).value;
  }
  });
  function Highlighting(){
    var markdowns = document.getElementsByClassName('markdown');
    for(var i=0;i<markdowns.length;i++){
       if(markdowns[i].innerHTML) markdowns[i].innerHTML =marked(markdowns[i].innerHTML);
    }
  }
  window.addEventListener('DOMContentLoaded', Highlighting, false);
  window.addEventListener('load', Highlighting, false);
</script>

<!-- comment.js -->
<script src="path_to_comment_js/comment.js"></script>
```

### 1. Create Github Repo

Create a Github repo for hosting your comments.

### 2. Register an OAuth App

To avoid abused API calling, Github API (and the same as OSChina API) has [Rate Limiting](https://developer.github.com/v3/#rate-limiting). To enlarge the limit you can register an OAuth application.

* [Register a Github OAuth application](https://github.com/settings/applications/new) .
* [Register an OSChina OAuth application(alternatively)](https://git.oschina.net/oauth/applications/new).

After that, you will get a client id as well as a client secret.

> Tips: When you registering the App, the Authorization callback URL can be assigned as your site address. For example http://hahack.com .

### 3. Comment Thread

In this section I will describe on how to generate a comment thread of a certain page for your site.

Firstly, write a div for placing your comment thread.

``` html
<div id="comment-thread"></div>
```

Secondly, write a div for placing a spinning indicator(optional):

``` html
<div id="loading-spin"></div>
```

Thirdly, call `getComments()` function to fetch comment thread and display them to the page.

``` js
<script type="text/javascript">
   var opt = {
       type: "github",
	   user: "wzpan",
	   repo: "comment.js",
	   no_comment: "No comments yet. Press the button and go to comment now!",
	   go_to_comment: "Go to comment",
	   issue_id: "1",
	   btn_class: "btn",
	   comments_target: "#comment-thread",
	   loading_target: "#loading-spin",
	   client_id: "xxxxxx",
	   client_secret: "xxxxxx"
   };
   getComments(opt);
</script>
```

parameters explaning:

* `type`: the site as the backend. Currently supports Github and OSChina.
* `user`: your site's user account.
* `repo`: your repo for comment issue tracking.
* `no_comment`: tips when no comments are fetched.
* `go_to_comment`: text of the "go to comment" button.
* `issue_title`: the corresponding issue title to your page. Alternatively you can use `issue_id`.
* `issue_id`: the corresponding issue id to your page. Alternatively you can use `issue_title`.
* `btn_class`: css class name for the "go to comment" button.
* `comments_target`: target for displaying comment thread.
* `loading_target`(optional): target for displaying loading a spinning indicator.
* `client_id`(optional but recommended): the client id of your OAuth App.
* `client_secret`(optional but recommended): the client secret of your OAuth App.

example(You won't see this from README): 

<div id="comment-thread"></div>
<div id="loading-spin"></div>

### 4. Recent Comment List

In this section I will describe on how to generate a recent comment list for your site.

Firstly, write a div for placing the recent comment list.

``` html
<div id="recent-comments"></div>
```

Secondly, call `getRecentCommentsList()` function to fetch a comment list and display them to the page.

``` js
<script type="text/javascript">
   var opt = {
       type: "github",
	   user: "wzpan",
	   repo: "comment.js",
	   recent_comments_target: "#recent-comments",
	   count: 5,
	   client_id: "xxxxxx",
	   client_secret: "xxxxxx"
   };
   getRecentCommentsList(opt);
</script>
```

parameters explaning:

* `type`: the site as the backend. Currently supports Github and OSChina.
* `user`: your site's user account.
* `repo`: your repo for comment issue tracking.
* `recent_comments_target`: target for displaying recent comment list.
* `count`(optional): the maximize length of the comment list. Default value is 5.
* `client_id`(optional but recommended): the client id of your OAuth App.
* `client_secret`(optional but recommended): the client secret of your OAuth App.

example(You won't see this from README):

<div id="recent-comments"></div>

## Browser support

All of the above browsers should support using comment.js, although I haven't fully test all of them.

* [x] Chrome
* [x] Safari
* [x] Firefox
* [x] IE8+

## How to contribute

* A Github star outweights 1000 dollars(一星胜千元).
* Pull requests are always welcome.
* Your donation will encourage me to improve this project. Supports Alipay & Wechat Pay.

| Alipay | Wechat Pay |
| ------ | --------- |
| <img src="http://7xj89i.com1.z0.glb.clouddn.com/ali_pay_01.jpg" height="248px" width="164px" title="Alipay" style="display:inherit;"/> | <img src="http://7xj89i.com1.z0.glb.clouddn.com/wechat_pay_02.png" height="248px" width="164px" title="Wechat Pay" style="display:inherit;"/> |


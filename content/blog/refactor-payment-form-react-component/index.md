---
title: 'Refactor payment form'
tags: ["react", ]
published: true
date: '2019-12-09'
---

Some rules to write React component:
<br>

Bad
```
<div>
  <Article></Article>
<div>
```
<br>

Good
```
<ArticleWrapper>
  <Article></Article>
</ArticleWrapper>
```

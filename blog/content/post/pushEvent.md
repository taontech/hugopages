---
title: "PushEvent"
date: 2022-03-17T14:57:13+08:00
---

### webhook 总是失败
应该是插件webhook的问题，如果已经存在git的仓库，则启动caddy时就会报错：没有设置仓库。删除掉已经同步的git路径，则可以成功

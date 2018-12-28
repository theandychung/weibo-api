# Weibo-api

![Python Version](https://img.shields.io/badge/python-2.7%20%7C%203.5%20%7C%203.6%20%7C%203.7-blue.svg)
![Build Version](https://img.shields.io/badge/version-v0.0.5-orange.svg)

[Sina Weibo](https://www.weibo.com) is a is a Chinese microblogging (weibo) website similar to [Twitter](https://twitter.com/). This api is a python package allowing users to fetch weibo posts without logging into any account.

Note: aggressively fetching from weibo will result an IP ban.

Note: I recreate this repo because the original one has never been updated. Hopefully this modified repo will be merged back one day.

一个免登陆获取新浪微博数据的Python库，简单易用

---

## Simple Example Code

```python
from weibo_api.client import WeiboClient
client = WeiboClient()

p = client.people('5623741644')  # 用户ID，后期加可以根据昵称创建用户
print(type(p.name))
print(u"用户名：{}".format(p.name))
print(u"用户简介：{}".format(p.description))
print(u"他关注的用户数：{}".format(p.follow_count))
print(u"关注他的用户数：{}".format(p.followers_count))
print(u"他最近发布的微博：")
print("==================================================")
# page(1) 表示获取第一页的内容，
# 可以用all()方法获取所有内容(对于非必要情况，不要获取全部，因为对于有大量内容的用户，需要进行大量网络请求)
for status in p.statuses.page(1):    
    print(u"微博动态：{}".format(status.id))
    print(u"发布时间：{}".format(status.created_at))
    if status.isTop is not None:
        print(u"置顶")
    print(u"微博内容概要：{}".format(status.text))
    print(u"转发数：{}".format(status.reposts_count))
    print(u"点赞数：{}".format(status.attitudes_count))
    print(u"评论数：{}".format(status.comments_count))
    print(u"发布于：{}".format(status.source))
    print("==================================================")

print('他的粉丝：')
print('#'*50)
for follower in p.followers.page(1):
    print(follower.name)

print('他关注的用户：')
print('#'*50)
for follow in p.follows.page(1):
    print(follow.name)

print('他的文章：')
print('#'*50)
for article in p.articles.page(1):
    print(article.text)
```
### Proxy
Configure proxy is the same way we configure any request method. Please read [python requests](http://docs.python-requests.org/en/master/user/advanced/#proxies) for more information.
```Python
from weibo_api.client import WeiboClient
proxy_ip = {
  'http': 'http://10.10.1.10:3128',
  'https': 'http://10.10.1.10:1080',
}
client = WeiboClient(proxies=proxy_ip)

```
---

## Installing

```
pip install weibo-api
```
---

## TODO

- [ ] 搜索接口
- [ ] 根据用户昵称创建用户
- [x] 头条文章获取
- [ ] 文章评论
- [ ] 文档
- [x] 测试

# Instagram 克隆器 [![构建状态](https://travis-ci.org/huaying/instagram-crawler.svg?branch=master)](https://travis-ci.org/huaying/instagram-crawler)

以下是你可以用此程序做的事情：
- 不使用 Instagram API 获取 Instagram 帖子/个人资料/话题标签数据。`crawler.py`
- 自动点赞帖子。`liker.py`

由于 Instagram 网站的更新，此克隆器可能会失败。如果遇到任何问题，请联系我。

## 安装
1. 确保你已安装 Chrome 浏览器。
2. 下载 [chromedriver](https://sites.google.com/a/chromium.org/chromedriver/) 并将其放入 bin 文件夹：`./inscrawler/bin/chromedriver`
3. 安装 Selenium：`pip3 install -r requirements.txt`
4. `cp inscrawler/secret.py.dist inscrawler/secret.py`

## 用户认证
1. 打开 `inscrawler/secret.py` 文件。
2. 将 `username` 和 `password` 变量的值更改为与你的 Instagram 帐户对应的值。
````
username = 'my_ig_username'
password = '***********'
````

## 克隆器
### 用法
```
位置参数(positional arguments):
  mode
    选项(options): [posts, posts_full, profile, hashtag]

可选参数(optional arguments):
  -n NUMBER, --number NUMBER
                        返回的帖子数量
  -u USERNAME, --username USERNAME
                        Instagram 的用户名
  -t TAG, --tag TAG     Instagram 的标签名称
  -o OUTPUT, --output OUTPUT
                        输出文件名（json 格式）

  --debug               查看程序如何自动化浏览器

  --fetch_comments      获取评论
  # 如果评论太多，打开此标志可能需要永远获取数据。

  --fetch_likes_plays   获取点赞/播放数量

  --fetch_likers        获取所有点赞者
  # Instagram 可能对获取点赞者有速率限制。如果点赞太多，打开此标志可能需要永远获取数据。

  --fetch_mentions      获取在标题/评论中被提及的用户（以 @ 开头）

  --fetch_hashtags      获取标题/评论中的话题标签（以 # 开头）

  --fetch_details       获取用户名和照片标题
  # 仅适用于“hashtag”搜索

```


### 示例
```
python crawler.py posts_full -u cal_foodie -n 100 -o ./output
python crawler.py posts_full -u cal_foodie -n 10 --fetch_likers --fetch_likes_plays
python crawler.py posts_full -u cal_foodie -n 10 --fetch_comments
python crawler.py profile -u cal_foodie -o ./output
python crawler.py hashtag -t taiwan -o ./output
python crawler.py hashtag -t taiwan -o ./output --fetch_details
python crawler.py posts -u cal_foodie -n 100 -o ./output # deprecated
```
1. 选择模式 `posts`，你将获得每个帖子的 URL、标题、第一张照片；选择模式 `posts_full`，你将获得每个帖子的 URL、标题、所有照片、时间、评论、点赞数和浏览数。模式 `posts_full` 将比模式 `posts` 花费更多的时间。 **[`posts` 已弃用。对于最近的帖子，没有快速获取帖子标题的方法]**
2. 如果不指定帖子数量 `-n`, `--number`，则返回默认的 100 个话题标签帖子（模式：hashtag）和所有用户的帖子（模式：posts）。
3. 如果不指定帖子的输出路径 `-o`, `--output`，则将结果打印到控制台。
4. 如果帖子数量超过 1000 个左右，获取数据需要更长的时间，因为 Instagram 已经设置了数据请求的速率限制。
5. 如果用户有超过 10000 个帖子，请不要使用此 repo 克隆 Instagram。

`posts` 的数据格式：
![screen shot 2018-10-11 at 2 33 09 pm](https://user-images.githubusercontent.com/3991678/46835356-cd521d80-cd62-11e8-9bb1-888bc32af484.png)

`posts_full` 的数据格式：
<img width="1123" alt="Screen Shot 2019-03-17 at 11 02 24 PM" src="https://user-images.githubusercontent.com/3991678/54510055-1c4f4080-4909-11e9-8d06-8c35a08fb74e.png">

## 点赞器
![点赞器预览](https://user-images.githubusercontent.com/3991678/41560884-4bbd42d2-72fd-11e8-8d56-84e7cf7187cd.gif)


在 `secret.py` 中设置你的用户名/密码，或将其设置为环境变量。

### 用法
```
位置参数(positional arguments):
  tag

可选参数(optional arguments):
  -n NUMBER, --number NUMBER (default 1000)
                        要点赞的帖子数量
```

### 示例
```
python liker.py foodie
```

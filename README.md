# 说明：在下当前直接clone项目使用，原项目地址在下面。感谢作者大大的开源，如果有冒犯到可以联系我删除该项目哟。
# 博客记录自己的一些生活和工作，技术文章并未写明参考文章出处，如果作者大大不开心可以联系我删除文章哟。
##[点我查看中文说明/Click here for Chinese instructions](https://github.com/bit-ranger/blog/blob/gh-pages/README_zh_CN.md)

# Blog Address

<https://bit-ranger.github.io/blog/>


# Must Modify

## 1.swiftype

This service provides the on-site search function.

Service address: <https://swiftype.com/>.

Documentation: <https://swiftype.com/documentation/site-search/crawler-quick-start/>

After the setup is complete， you need to modify the `swiftype.searchId` in `_config.yml`.

In your swiftype engine, go to `Install Search`, you will find the `swiftype.searchId`.

```html
<script type="text/javascript">
...
...
  _st('install','swiftype.searchId','2.0.0');
</script>
```

## 2.gitment

This service provides the comment function.

Service address： <https://github.com/imsun/gitment>.

After the setup is complete， you need to modify the `gitment`  in `_config.yml`.

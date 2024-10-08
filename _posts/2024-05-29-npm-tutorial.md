---
title: npm tutorial
date: 2024-05-29
---

# NPM Tutorial

## CLI

```sh
npm install <package>
npm outdated         # 先查询有哪些包有更新
npm update <package> # 更新指定包
npm install -g npm-check # 先全局安装 npm-check
npm-check -u             # 查询当前项目下的包更新
npm-check -u -g          # 查询全局安装的包更新
```

### 查看

查看包被谁依赖：

```sh
npm ls inflight --depth infinity
```

## Config

**网络配置**

```shell
# 设置代理（默认使用终端的全局代理http_proxy和https_proxy）
npm config set proxy http://server:port
npm config set https-proxy http://server:port

# 比如：
npm config set proxy http://0.0.0.0:7890
npm config set https-proxy http://0.0.0.0:7890
```

我不建议换源以及使用其他工具（比如cnpm），可能会遇到 electron 不能拉取或者成功 `npm i` 后但出现运行的问题。

## Package

NPM的项目会包含 `package.json` 配置文件。


```json
{
  "name": "project name",
  "version": "0.0.0",
  "scripts": {
	// ...
  },
  "private": true,
  "dependencies": {
	// ...
  },
  "devDependencies": {
	// for example
    "typescript": "~4.8.2"
	// ...
  }
}
```


`^` 表示：匹配最近的一个大版本比如1.0.2 将会匹配到1.x.x，但不包括2.x.x
`~` 表示：匹配最近的小版本比如~1.0.2将会匹配1.0.x版本，但不匹配1.1.0
# M3U8

## 魔改的地方
- 原作者项目地址: (https://github.com/llychao/m3u8-downloader)
- 修改命令行 默认用法: m3u8 url/of/list.m3u8
- 2021-07-14: 添加: http下载的User-Agent，有些站点会要求这个，见 tool/http.go

## 以下为原版内容

# m3u8-downloader

golang 多线程下载直播流m3u8格式的视屏，跨平台。 你只需指定必要的 flag (`u`、`o`、`n`、`ht`) 来运行, 工具就会自动帮你解析 M3U8 文件，并将 TS 片段下载下来合并成一个文件。


## 功能介绍

1. 下载和解析 M3U8
2. 下载 TS 失败重试 （加密的同步解密)
3. 合并 TS 片段

> 可以下载岛国小电影  
> 可以下载岛国小电影  
> 可以下载岛国小电影    
> 重要的事情说三遍......

## 效果展示
![demo](./demo.gif)

## 参数说明：

```
- u M3U8 地址
- o 自定义文件名, 默认 output
- n 下载协程并发数，默认 16
- ht 设置getHost的方式（共两种 apiv1 和 apiv2）, 默认 apiv1
- c 自定义请求cookie, 默认空
```

默认情况只需要传`u`参数,其他参数保持默认即可。 部分链接可能限制请求频率，可根据实际情况调整 `n` 参数的值。

## 下载

已经编译好的平台有： [点击下载](https://github.com/llychao/m3u8-downloader/releases)

- windows/amd64
- linux/amd64
- darwin/amd64

## 用法

### 源码方式

```bash
自己编译：go build -o m3u8-downloader
简洁使用：./m3u8-downloader  -u=http://example.com/index.m3u8
完整使用：./m3u8-downloader  -u=http://example.com/index.m3u8 -o=example -n=16 -ht=apiv1 -c="key1=v1; key2=v2"
```

### 二进制方式:

Linux 和 MacOS 和 Windows PowerShell

```
简洁使用：
./m3u8-downloader-v1.0.0-linux-amd64 -u=http://example.com/index.m3u8
./m3u8-downloader-v1.0.0-darwin-amd64 -u=http://example.com/index.m3u8 
.\m3u8-downloader-v1.0.0-windows-amd64.exe -u=http://example.com/index.m3u8

完整使用：
./m3u8-downloader-v1.0.0-linux-amd64 -u=http://example.com/index.m3u8 -o=example -n=16 -ht=apiv1 -c="key1=v1; key2=v2"
./m3u8-downloader-v1.0.0-darwin-amd64 -u=http://example.com/index.m3u8 -o=example -n=16 -ht=apiv1 -c="key1=v1; key2=v2"
.\m3u8-downloader-v1.0.0-windows-amd64.exe -u=http://example.com/index.m3u8 -o=example -n=16 -ht=apiv1 -c="key1=v1; key2=v2"
```

## 问题说明

1.在Linux或者mac平台，如果显示无运行权限，请用chmod 命令进行添加权限
```bash
 # Linux amd64平台
 chmod 0755 m3u8-downloader-v1.0.0-linux-amd64
 # Mac darwin amd64平台
 chmod 0755 m3u8-downloader-v1.0.0-darwin-amd64
 ```
2.下载失败的情况,请设置 -ht="apiv1" 或者 -ht="apiv2" （默认为apiv1）
```golang
func get_host(Url string, ht string) string {
    u, err := url.Parse(Url)
    var host string
    checkErr(err)
    switch ht {
    case "apiv1":
        host = u.Scheme + "://" + u.Host + path.Dir(u.Path)
    case "apiv2":
        host = u.Scheme + "://" + u.Host
    }
    return host
}
```

# OpenPandora

一个可更换后端反代的潘多拉。

You can use self-host reverse-proxy backend with this version. 

注：无论用哪种工具，最终都需要一个可以正常访问chatgpt官网的ip或者节点作为最终出口，这个出口可以是支持chatgpt的梯子或者买一个干净的居家IP。

这个版本的修改是让pandora不再捆绑使用fakeopen，可以更换为自己管理可控的反代服务器。

Note: Regardless of the tool used, in the end, an IP or node that can normally access the ChatGPT official website is required as the final exit.

The modification in this version allows Pandora to no longer be bundled with fakeopen, and it can be replaced with a reverse proxy server that you manage and control.



# 使用/Usage
### AccessToken方式（网页版）：

OpenPandora会依然使用 https://ai.fakeopen.com 作为默认后端反代，使用fakeopen你不需要配置梯子，但你的所有数据都会通过fakeopen转发；

OpenPandora可以切换使用开源的go-chatgpt-api，建立go-chatgpt-api服务之后，设置OpenPandora的环境参数 CHATGPT_API_PREFIX=http://[ip]:[port]/chatgpt 即可。

go-chatgpt-api地址：https://github.com/linweiyuan/go-chatgpt-api/tree/main


以下是一个简单的docker-compose构建：
```yaml
version: '3.3'
services:
    openpandora:
        container_name: openpandora
        volumes:
            - ./data:/data
        ports:
            - 3000:3000
        environment:
            - PANDORA_SERVER=0.0.0.0:3000
            - CHATGPT_API_PREFIX=http://go-chatgpt-api:8080/chatgpt
            - PANDORA_ACCESS_TOKEN=${TOKEN}
        depends_on:
            - go-chatgpt-api
        restart: unless-stopped
        image: axaxin/openpandora

    go-chatgpt-api:
        container_name: go-chatgpt-api
        image: linweiyuan/go-chatgpt-api
        expose:
            - 8080
        environment:
            - GIN_MODE=release
            - GO_CHATGPT_API_PROXY=[socks5|http]://[梯子ip]:[port]
        restart: unless-stopped
```

### API调用方式

设置环境变量OPENAI_API_PREFIX=http://[go-chatgpt-api的ip]:[port]/platform




## 体验地址
* 访问 [这里](http://chat.openai.com/api/auth/session) 拿 `Access Token`
* 也可以访问 [这里](http://ai.fakeopen.com/auth) 拿 `Access Token`
* `Access Token` 有效期 `14` 天，期间访问**不需要梯子**。这意味着你在手机上也可随意使用。

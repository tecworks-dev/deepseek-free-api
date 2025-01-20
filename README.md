# DeepSeek V3 Free Service

[![](https://img.shields.io/github/license/llm-red-team/deepseek-free-api.svg)](LICENSE)
![](https://img.shields.io/github/stars/llm-red-team/deepseek-free-api.svg)
![](https://img.shields.io/github/forks/llm-red-team/deepseek-free-api.svg)
![](https://img.shields.io/docker/pulls/vinlic/deepseek-free-api.svg)

Supports high-speed streaming output, multi-round conversations, online search, R1 deep thinking and silent deep thinking, zero-configuration deployment, and multi-channel token support.

Fully compatible with ChatGPT interface.

There are also the following ten free-apis welcome to pay attention to:

Moonshot AI (Kimi.ai) interface to API [kimi-free-api](https://github.com/LLM-Red-Team/kimi-free-api)

Zhipu AI (Zhipu Qingyan) interface to API [glm-free-api](https://github.com/LLM-Red-Team/glm-free-api)

StepChat interface to API [step-free-api](https://github.com/LLM-Red-Team/step-free-api)

Ali Tongyi (Qwen) interface to API [qwen-free-api](https://github.com/LLM-Red-Team/qwen-free-api)

Metaso API conversion [metaso-free-api](https://github.com/LLM-Red-Team/metaso-free-api)

ByteDance (Doubao) interface to API [doubao-free-api](https://github.com/LLM-Red-Team/doubao-free-api)

ByteDance (Jimeng AI) interface to API [jimeng-free-api](https://github.com/LLM-Red-Team/jimeng-free-api)

iFlytek Spark interface to API [spark-free-api](https://github.com/LLM-Red-Team/spark-free-api)

MiniMax (Conch AI) interface to API [hailuo-free-api](https://github.com/LLM-Red-Team/hailuo-free-api)

Emohaa API conversion [emohaa-free-api](https://github.com/LLM-Red-Team/emohaa-free-api)

## Table of contents

* [Disclaimer](#Disclaimer)
* [Effect Example](#Effect Example)
* [Access Preparation](#Access Preparation)
  * [Multiple account access](#Multiple account access)
* [Docker deployment](#Docker deployment)
  * [Docker-compose deployment](#Docker-compose deployment)
* [Render deployment](#Render deployment)
Vercel deployment
* [Native deployment](#Native deployment)
* [Recommended client](#Recommended client)
* [Interface list](#Interface list)
  * [Dialogue completion](#Dialogue completion)
  * [userToken survival detection](#userToken survival detection)
* [Notes](#Notes)
  * [Nginx Anti-Generation Optimization](#Nginx Anti-Generation Optimization)
  * [Token Statistics](#Token Statistics)
* [Star History](#star-history)
  
## Disclaimer

**The reverse API is unstable. It is recommended to go to DeepSeek official https://platform.deepseek.com/ to pay to use the API to avoid the risk of being banned. **

**This organization and individuals do not accept any financial donations or transactions. This project is purely for research, exchange and learning purposes! **

**For personal use only, no external services or commercial use are allowed to avoid causing service pressure to the official website, otherwise you will bear the risk yourself! **

**For personal use only, no external services or commercial use are allowed to avoid causing service pressure to the official website, otherwise you will bear the risk yourself! **

**For personal use only, no external services or commercial use are allowed to avoid causing service pressure to the official website, otherwise you will bear the risk yourself! **

## Effect Example

### Identity verification Demo

![Identity Verification](./doc/example-1.png)

### Multi-round dialogue Demo

![Multi-round dialogue](./doc/example-2.png)

### Network Search Demo

![Internet search](./doc/example-3.png)

## Access Preparation

Please make sure you are in China or have a server in China, otherwise you may not be able to use it after deployment because you cannot access DeepSeek.

Get the userToken value from [DeepSeek](https://chat.deepseek.com/)

Enter DeepSeek and start a random conversation. Then press F12 to open the developer tools. Find the value of `userToken` in Application > LocalStorage. This will be used as the Bearer Token value of Authorization: `Authorization: Bearer TOKEN`

![Get userToken](./doc/example-0.png)

### Multiple account access

Currently, there can only be *one* output per account at a time. You can provide multiple accounts' userToken values ​​and concatenate them using `,`:

`Authorization: Bearer TOKEN1,TOKEN2,TOKEN3`

The service will pick one of them each time it is requested.

## Docker Deployment

Please prepare a server with a public IP and open port 8000.

Pull the image and start the service

```shell
docker run -it -d --init --name deepseek-free-api -p 8000:8000 -e TZ=Asia/Shanghai vinlic/deepseek-free-api:latest
```

View service real-time log

```shell
docker logs -f deepseek-free-api
```

Restart the service

```shell
docker restart deepseek-free-api
```

Stop service

```shell
docker stop deepseek-free-api
```

### Docker-compose deployment

```yaml
version: '3'

services:
  deepseek-free-api:
    container_name: deepseek-free-api
    image: vinlic/deepseek-free-api:latest
    restart: always
    ports:
      - "8000:8000"
    environment:
      - TZ=Asia/Shanghai
```

### Render deployment

**Note: Some deployment areas may not be able to connect to deepseek. If the container log shows a request timeout or a connection failure, please switch to another area for deployment! **
**Note: The container instance of the free account will automatically stop running after a period of inactivity, which may cause a delay of 50 seconds or more on the next request. It is recommended to check [Render container keep alive](https://github.com/LLM-Red-Team/free-api-hub/#Render%E5%AE%B9%E5%99%A8%E4%BF%9D%E6%B4%BB)**

1. Fork this project to your GitHub account.

2. Visit [Render](https://dashboard.render.com/) and log in to your GitHub account.

3. Build your Web Service (New+ -> Build and deploy from a Git repository -> Connect your forked project -> Select the deployment area -> Select instance type as Free -> Create Web Service).

4. After the build is complete, copy the assigned domain name and concatenate the URL to access it.

Vercel Deployment

**Note: The request response timeout for Vercel free accounts is 10 seconds, but the interface response is usually longer, and you may encounter a 504 timeout error returned by Vercel! **

Please make sure that the Node.js environment is installed first.

```shell
npm i -g vercel --registry http://registry.npmmirror.com
vercel login
git clone https://github.com/LLM-Red-Team/deepseek-free-api
cd deepseek-free-api
vercel --prod
```

## Native deployment

Please prepare a server with a public IP and open port 8000.

Please install the Node.js environment and configure the environment variables first, and confirm that the node command is available.

Install Dependencies

```shell
npm i
```

Install PM2 for process daemon

```shell
npm i -g pm2
```

Compile and build. When you see the dist directory, the build is complete.

```shell
npm run build
```

Start the service

```shell
pm2 start dist/index.js --name "deepseek-free-api"
```

View service real-time log

```shell
pm2 logs deepseek-free-api
```

Restart the service

```shell
pm2 reload deepseek-free-api
```

Stop service

```shell
pm2 stop deepseek-free-api
```

## Recommended client

It is faster and easier to use the following secondary development client to access the free-api series projects, and support document/image uploading!

LobeChat [https://github.com/Yanyutin753/lobe-chat](https://github.com/Yanyutin753/lobe-chat) developed by [Clivia](https://github.com/Yanyutin753/lobe-chat)

ChatGPT Web [https://github.com/SuYxh/chatgpt-web-sea](https://github.com/SuYxh/chatgpt-web-sea) developed by [时光@](https://github.com/SuYxh)

## Interface List

Currently, the `/v1/chat/completions` interface compatible with OpenAI is supported. You can use the access interface with OpenAI or other compatible clients, or use online services such as [dify](https://dify.ai/) to access it.

### Dialogue completion

Chat completion API, compatible with OpenAI's chat-completions-api.

**POST /v1/chat/completions**

The Authorization header needs to be set:

```
Authorization: Bearer [userToken value]
```

Request data:
```json
{
    //model name
    // Default: deepseek
    // Deep thinking: deepseek-think or deepseek-r1
    // Online search: deepseek-search
    // Silent mode (do not output thinking process or online search results): deepseek-think-silent or deepseek-r1-silent or deepseek-search-silent
    // Deep thinking, but the thinking process is wrapped in <details> foldable tags (needs page support display): deepseek-think-fold or deepseek-r1-fold
    "model": "deepseek",
    // By default, multi-round conversations are implemented based on message merging. In some scenarios, this may result in reduced capabilities and is limited by the maximum number of tokens per round.
    // If you want to get a native multi-round conversation experience, you can pass in the id obtained from the previous round of messages to continue the context
    // "conversation_id": "50207e56-747e-4800-9068-c6fd618374ee@2",
    "messages": [
        {
            "role": "user",
            "content": "Who are you?"
        }
    ],
    // If you use streaming response, please set it to true, the default is false
    "stream": false
}
```

Response data:
```json
{
    "id": "50207e56-747e-4800-9068-c6fd618374ee@2",
    "model": "deepseek",
    "object": "chat.completion",
    "choices": [
        {
            "index": 0,
            "message": {
                "role": "assistant",
                "content": " I am DeepSeek Chat, an intelligent assistant developed by DeepSeek, which aims to provide information query, conversation and question answering services through natural language processing and machine learning technologies."
            },
            "finish_reason": "stop"
        }
    ],
    "usage": {
        "prompt_tokens": 1,
        "completion_tokens": 1,
        "total_tokens": 2
    },
    "created": 1715061432
}
```

### userToken survival detection

Check whether the userToken is alive. If it is alive, it is true, otherwise it is false. Please do not call this interface frequently (less than 10 minutes).

**POST /token/check**

Request data:
```json
{
    "token": "eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9..."
}
```

Response data:
```json
{
    "live": true
}
```

## Notes

### Nginx anti-generation optimization

If you are using Nginx reverse proxy deepseek-free-api, please add the following configuration items to optimize the output effect of the stream and the user experience.

```nginx
# Turn off proxy buffering. When set to off, Nginx will immediately send client requests to the backend server and immediately send the response received from the backend server back to the client.
proxy_buffering off;
# Enable chunked transfer encoding. Chunked transfer encoding allows the server to send data in chunks for dynamically generated content without knowing the size of the content in advance.
chunked_transfer_encoding on;
# Turn on TCP_NOPUSH, which tells Nginx to send as much data as possible before sending it to the client. This is usually used in conjunction with sendfile to improve network efficiency.
tcp_nopush on;
# Turn on TCP_NODELAY, which tells Nginx to send small packets immediately without delay. In some cases, this can reduce network latency.
tcp_nodelay on;
# Set the timeout for maintaining the connection, here it is set to 120 seconds. If there is no further communication between the client and the server during this period of time, the connection will be closed.
keepalive_timeout 120;
```

### Token Statistics

Since the inference side is not in deepseek-free-api, tokens cannot be counted and will be returned as fixed numbers.

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=LLM-Red-Team/deepseek-free-api&type=Date)](https://star-history.com/ #LLM-Red-Team/deepseek-free-api&Date)

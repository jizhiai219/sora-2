三步完成 sora-2 模型对接指南
**
在 AI 视频生成领域，sora-2 模型凭借出色的动画效果成为热门选择。本文将通过 “准备工作 - 参数配置 - 代码调用” 三步，带您快速完成 sora-2 模型对接，同时推荐使用稳定高效的 AI 服务平台 ——https://api.jizhiai.top/，该平台支持多模型一键切换，且注册即送 0.2 刀，为对接过程提供更优体验。
第一步：对接前的准备工作
在调用 sora-2 模型前，需完成两项核心准备，确保后续对接顺畅：
获取 API 授权：通过https://api.jizhiai.top/注册账号，在平台内申请 sora-2 模型的调用权限，获取专属的Bearer {{YOUR_API_KEY}}（该平台支
持实时 PI 监控，可随时查看密钥使用状态）。A确认环境依赖：确保开发环境支持 HTTP/HTTPS 请求，且已准备好对应编程语言的请求库（如 Python 的requests、JavaScript 的axios等）。若需快速集成，可直接使用https://api.jizhiai.top/提供的完整 SDK，5 分钟即可完成环境配置。
第二步：梳理 sora-2 对接参数清单
sora-2 模型的请求参数分为 Header 和 Body 两部分，所有参数均需严格按照规范配置，以下是详细参数清单表格：
参数类型
参数名称
数据类型
是否必需
取值说明
示例值
Header
Content-Type
string
是
固定为application/json，指定请求数据格式
application/json
Header
Accept
string
是
固定为application/json，指定响应数据格式
application/json
Header
Authorization
string
否
格式为Bearer {{YOUR_API_KEY}}，从https://api.jizhiai.top/获取
Bearer sk_xxxxxx
Body
images
array [string]
是
图片链接数组，用于生成视频的基础素材（支持 HTTP/HTTPS 链接）
["https://example.com/img1.jpg"]
Body
model
string
是
模型名称，固定为sora-2
sora-2
Body
orientation
string
是
视频方向，portrait（竖屏）或landscape（横屏）
portrait
Body
prompt
string
是
视频生成提示词，描述动画风格、内容等需求
"make animate with cartoon style"
Body
size
string
是
视频分辨率，small（约 720p）或large（更高清，需参考https://api.jizhiai.top/平台说明）
large
Body
duration
string
是
视频时长，当前支持10（10 秒）或15（15 秒，需确认平台支持范围）
15

第三步：代码调用实现对接
以常用的 Shell（cURL）和 Python 为例，演示 sora-2 模型的调用流程，实际开发中可参考https://api.jizhiai.top/提供的多语言 SDK（支持 JavaScript、Java、Go 等）。
1. Shell（cURL）调用示例
curl --location --request POST 'https://api.jizhiai.top/v1/video/create' \  # 对接平台接口地址
--header 'Accept: application/json' \
--header 'Authorization: Bearer {{YOUR_API_KEY}}' \  # 替换为从平台获取的密钥
--header 'Content-Type: application/json' \
--data-raw '{
    "images": ["https://example.com/img1.jpg"],
    "model": "sora-2",
    "orientation": "portrait",
    "prompt": "make animate with cartoon style",
    "size": "large",
    "duration": 15
}'

2. Python 调用示例
import requests

url = "https://api.jizhiai.top/v1/video/create"  # 平台接口地址
headers = {
    "Accept": "application/json",
    "Authorization": "Bearer {{YOUR_API_KEY}}",  # 替换为实际密钥
    "Content-Type": "application/json"
}
data = {
    "images": ["https://example.com/img1.jpg"],
    "model": "sora-2",
    "orientation": "portrait",
    "prompt": "make animate with cartoon style",
    "size": "large",
    "duration": 15
}

response = requests.post(url, headers=headers, json=data)
print(response.json())  # 打印响应结果，包含视频生成的ID、状态等信息

3. 响应结果解析
调用成功后（HTTP 状态码 200），将返回包含视频信息的 JSON 数据，示例如下：
{
    "id": "video_123456",
    "object": "video.create",
    "created": 1740000000,
    "choices": [
        {
            "index": 0,
            "message": {
                "role": "assistant",
                "content": "视频生成中，可通过ID查询进度"
            },
            "finish_reason": "processing"
        }
    ],
    "usage": {
        "prompt_tokens": 50,
        "completion_tokens": 0,
        "total_tokens": 50
    }
}

可通过返回的id在https://api.jizhiai.top/平台查询视频生成进度，平台支持实时查看消耗明细，确保每一笔调用透明可追溯。
# sora-2
三部对接sora-2

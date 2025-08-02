# ddddocr-server

基于 [ddddocr](https://github.com/sml2h3/ddddocr) 的轻量级验证码识别接口服务，适用于自动化脚本、爬虫识别验证码等场景。

## 🚀 快速开始

### 拉取并运行 Docker 容器

```bash
docker run --name ddddocr-server -p 23456:80 -d jeanhua/ocr-server:latest
```

- 容器名称：`ddddocr-server`
- 本地访问地址：`http://localhost:23456/ocr`

---

## 📡 接口使用说明

### 请求地址

```
POST http://localhost:23456/ocr
```

### 请求方式

`POST`，请求体为 JSON 格式。

### 请求参数

| 参数名 | 类型     | 必填 | 说明                                               |
|-----|--------|----|--------------------------------------------------|
| img | String | 是  | 图片 Base64 编码字符串（不带前缀，如 `data:image/png;base64,`） |

### 请求示例

```json
{
  "img": "/9j/4AAQSkZJRgABAQEASABIAAD/2wBDAAMCAgMCAgMDAwMEAwMEBQgFBQQEBQoHBwYIDAoMDAsKCwsNDhIQDQ4RDgsLEBYQERMUFRU..."
}
```

### 返回格式

```json
{
  "result": "dbcd"
}
```

---

### 示例调用

#### 使用 curl 命令（Linux/macOS）

```bash
base64_str=$(base64 -w 0 captcha.jpg)  # macOS 用 `-b 0` 替代 `-w 0`

curl -X POST http://localhost:23456/ocr \
  -H "Content-Type: application/json" \
  -d "{\"img\":\"${base64_str}\"}"
```

#### 使用 Python 调用示例

```python
import base64
import requests

with open("captcha.jpg", "rb") as f:
    img_base64 = base64.b64encode(f.read()).decode()

payload = {"img": img_base64}
response = requests.post("http://localhost:23456/ocr", json=payload)
print(response.json())
```

---

## 📦 容器管理命令

查看容器日志：

```bash
docker logs -f ddddocr-server
```

停止容器：

```bash
docker stop ddddocr-server
```

删除容器：

```bash
docker rm -f ddddocr-server
```

---

## 📌 注意事项

- 本服务使用 `ddddocr` 默认模型，支持识别常见数字和字母验证码。
- 不支持复杂背景干扰、旋转扭曲、多行文字等高难度场景。
- Base64 图片字符串请不要带 `data:image/...;base64,` 前缀。
- 如需更高识别率或定制模型，请自行使用 `ddddocr` 项目进行训练和部署。

---

## 🔗 相关链接

- OCR 引擎项目： [ddddocr](https://github.com/sml2h3/ddddocr)
- Docker 镜像仓库： [jeanhua/ocr-server](https://hub.docker.com/r/jeanhua/ocr-server)

---

## 🛠️ 开发者说明

本服务适用于快速部署 OCR 能力，适合自动化平台、验证码识别任务等场景。如需扩展支持其他验证码类型、增强识别率、添加多线程支持等功能，建议在
`ddddocr` 基础上进行二次开发。

---

> 📄 文档内容由 AI 协助生成。

---
name: phone-number-lookup
version: 1.0.0
author: Hermes Agent
license: MIT
description: Use when 判断美国号码是否虚拟号或查运营商归属。
metadata:
  hermes:
    tags: [phone, lookup, voip, osint]
    related_skills: [grounded-citations]
---

# 号码查询（虚拟号 / 运营商 / 归属地）

用 PhoneValidator v4 API 判断一个美国/加拿大号码是手机、固话还是 **VoIP 虚拟号**，并给出当前运营商和归属城市。

## When to Use（什么时候用）

- 用户发来一个号码，问「是不是虚拟号 / 谁的 / 哪里的」
- 判断短信验证码来源、陌生来电、批量外呼号码

## 现成工具（首选，别每次手敲 curl）

- 网页：`http://192.168.1.9:9107/`（服务 `hermes-phonelookup.service`，代码 `/vol1/1000/Docker/phonelookup/app.py`）
- 接口：`GET http://192.168.1.9:9107/api?phone=6036084257` → JSON
  返回字段：`line_type`（VOIP / CELL PHONE / LANDLINE）、`virtual`、`carrier`、`location`、`fake`

## 直接调上游 API

```bash
KEY=$(grep -oP '(?<=API_KEY=).*' ~/.hermes/workspace/phonevalidator.env)
curl -s --proxy http://127.0.0.1:2080 \
  "https://api.phonevalidator.com/api/v4/phonesearch?apikey=$KEY&phone=6036084257&type=basic&region=1"
```

- **`type` 和 `region` 都是必填**，少一个就返回 `501 BAD REQUEST - MISSING PARAMETERS`。
- `type=basic`（线路类型+运营商+假号标记）| `type=fake`（只查假号）；`region=1` 美国、`2` 加拿大、`3` 加勒比。
- 号码去掉标点即可；非美加必须带国家码。速率限制 25 次/秒。
- 每次查询计费约 **$0.004**（`Cost` 字段会回传），演示号 `7029270000` 不计费。
- key 存在 `~/.hermes/workspace/phonevalidator.env`（600）；**别 echo key**。

## 判读要点

- `LineType=VOIP` = 虚拟号；常见承载商 `TELNYX`、`BANDWIDTH.COM`、`LEVEL 3`、`ONVOY`、`PEERLESS` —— 这些是批发语音平台，Twilio/接码平台的上游。
- `LineType=CELL PHONE` + `PhoneCompany=T-MOBILE/VERIZON/AT&T` = 真手机号。
- `FakeNumber=YES` 才是伪造号；**虚拟号 ≠ 假号**（号码真实分配，只是跑在 IP 线路上）。
- **号段查询会被携号转网误导**：查「号段归属」类网站（如 603-608 显示 Sprint）只能说明原始分配，现在可能已 port 到 Telnyx。以 API 的 `PhoneCompany` 为准。
- 报结果时给：virtual 判断 + 运营商 + 归属地 + 一句「能收短信、常被用于注册/群发」的实际含义。

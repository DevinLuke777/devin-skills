# 美客多开放平台 API（接入自建报表/工具时用）

用户问「申请了 API 是不是就能接实时数据 / 用国内 IP 会不会封号」时按本文件答。结论要分层：**哪些数据真能实时拿、哪些拿不到、怎么接才不改动他的登录环境**。

## 1. 能拿到 vs 拿不到

| 数据 | 能否通过 API | 说明 |
|---|---|---|
| 订单/销售（金额、付款、运费、买家、SKU、件数） | ✅ `GET /orders/search` | 可替代后台 Vendas 导出；取消/退款会回写，需定时重拉而不是只拉一次 |
| 商品、价格、状态 | ✅ `/items`、`/items/{id}` | 写操作（改价/改库存/发货）不要用，风控与误操作风险都在写侧 |
| 库存（含 Full） | ✅ `/inventories`、Fulfillment API | Full 相关要账号资料完整并授予对应 scope |
| 访问量 | ⚠️ `/items/{id}/visits` | 可替代「商品表现」报表的访问数；**曝光/转化率的完整口径仍要靠平台报表** |
| 发货/物流轨迹 | ✅ `/shipments` | |
| 问答/消息/评价 | ✅ `/questions`、`/messages`、`/reviews` | |
| **广告花费 / ACOS / 广告销售额** | ❌（个人应用基本拿不到） | Mercado Ads API 需单独申请，通常只批认证合作伙伴 → **广告报表继续人工导出** |
| **成本、利润表** | ❌ | 采购成本/BP 表是卖家的私有数据，API 没有 |
| **资金结算流水** | ❌ 另一套 | Mercado Pago API，独立 OAuth |

## 2. OAuth 2.0 要点（别踩这几个）

- 授权页 `auth.mercadolivre.com.br` → 回调带 `code` → 换 `access_token` + `refresh_token`。
- **access_token 6 小时过期；refresh_token 一次性、6 个月** → 必须服务端定时刷新并**原子回写**新 refresh_token，人肉刷新一定会掉链。
- 回调地址必须公网 HTTPS：可用 `https://ml.devinluke.top/mlapi/callback`（与主站同一 nginx 反代）。
- 多站点同一套 API，用 `site_id`（MLB/MLM/MLC）区分。

## 3. 申请 App 的表单（葡语，照抄）

- Nome：`Painel Interno Equipe Zero`
- Nome curto：**全局唯一**，实测 `Equipe` 直接被拒（「Esta aplicação já está em uso」）。按顺序试：`ezpainel` → `ezpainelbr` → `equipezero-painel` → `ez-relatorios` → `equipezero-mlb`。只能小写字母/数字/连字符；这名字只给内部识别，不展示给买家，别纠结好听
- Descrição：`Painel interno de gestão da loja Equipe Zero: leitura de pedidos, estoque e visitas para relatórios próprios. Uso exclusivo do vendedor (1 conta).`
- Propósito：**Pessoal**（选 Negócios 会被当第三方 SaaS 走审核）
- Usuários：1 a 10
- Logo：512×512 PNG（本地生成：圆角方圆底 `#3483fa` + 白字 `EZ` + 底部 `#ffe600` 细条）

自查：用**自己的卖家账号**登录 developers 建 App、授权给自己，是官方支持路径，不需要合作伙伴审核。

### 建完后的「Configuração e scopes」页（逐项）

- `URI de redirecionamento`：`https://ml.devinluke.top/mlapi/callback`（必须点 ⊕ 真正加进列表，只填在框里不算）；多留一行空的没关系。
- Fluxos OAuth：`Authorization Code` ✅、`Refresh Token` ✅（**不勾就 6 小时后掉线，得反复人工授权**）、`Client Credentials` 取消、`PKCE` 不勾。
- Negócios：`Mercado Livre` ✅、`VIS` 不勾。
- Permissões 一律只给 `Leitura`：`Usuários`、`Métricas do negócio`、`Venda e envios de um produto`、`Publicidade de um produto`；其余（Comunicações / Publicação / Faturamento / Promoções）保持 `Sem acesso`。写权限一概不给。
- `Tópicos`（webhook）全部不勾、`URL de retornos de chamada de notificação` 留空：我们用定时轮询，开推送还要验签+去重，只会多一层故障。
- 最下面必须勾 `Aceito os Termos e Condições` 才存得住。
- ❌ **不要点 `Publicar aplicação` / `Solicitar certificação`**：一发布就按第三方服务审核，开发模式（`Aplicativo não certificado`）自己用完全够。页面上的 `Configuração de segurança 70` 只是开发者安全评分，与接口权限无关，可忽略。

### Client ID / Client Secret 在哪（这一点最费时间）

- App 卡片上直接显示 **`Client ID`**（纯数字）。
- App 卡片的 `⋮` 菜单只有 `Editar / Administrar permissões / Apagar` —— **没有 Credenciais**，别在那儿找。
- Secret 在：`⋮` → **`Editar`** → 页面底部 **`Configuração da aplicação`** 里的 **`Chave secreta`**，点旁边眼睛 👁 才显示，**只显示一次**，先让用户存进备忘录再发。忘了就 `Gerar novo secret` 重新生成（旧作废，此时还没启用就无所谓）。

## 4. 用国内 IP 会不会封号（他真正的顾虑）

判断口径：**封号看的是违规行为与登录行为，不是 IP 数量本身**。

- API 用 token，**不产生登录会话**，不改变浏览器登录环境 → 接入 API 反而减少他去后台登录的次数。
- 高危的是：同一账号短时间跨地区跳动登录、多账号共用 IP、换新 IP 后立刻做大操作（改价/改密/提现）。
- 实测结论（可复述）：`api.mercadolibre.com` 从大陆 IP 直连**能通**，未带 token 时返回 `403 PA_UNAUTHORIZED_RESULT_FROM_POLICIES`（策略拦截，不是地域封锁），走代理同样 403。
- 稳妥接法：**API 出口固定一条线路**（不轮换）+ **只做 GET 只读**（订单/库存/访问/发货）+ token 自动续 + 先跑 1~2 周观察。写操作（改价/改库存/发货）继续人工。

## 5. 落地顺序（建议）

1. 先只接**库存只读**（最无害），跑两周；
2. 再加订单只读，用它替代 Vendas 上传；
3. 广告报表、成本表仍然人工/Excel —— 别承诺「全自动」。

## 6. NAS 侧的授权回调服务（已落地，`mlapi_server.py`）

- 服务：`~/美客多报表/mlapi_server.py`，systemd 单元 `mlb-mlapi`（`User=1000/Group=1001`，端口 8305）；数据目录 `~/mlb-api/`（`code.json` / `token.json` / `app.json`，600 权限）。
- 路由：nginx `location /mlapi/ { auth_basic off; proxy_pass http://172.17.0.1:8305/; }` —— **必须 `auth_basic off`**，否则美客多跳回来会卡在站点口令弹窗。
- 端点：`GET /callback?code=`（存 code + 回一个中文“授权成功”页）、`GET /health`、`GET /token`（只报状态/剩余秒数，不吐 token 本体）、`POST /exchange`（用 `app.json` 里的 app_id/secret 换 token）、`POST /refresh`。
- 授权顺序：先把 App ID + Secret 落到 `app.json` → 再把授权链接给用户 → 他点完 `Permitir` 页面自动跳到 `/mlapi/callback` → 立刻 `POST /exchange` 换 token（**授权码 10 分钟有效、一次性**，过期只能重新授权）。


# Edge Sub

在 CloudFlare 的全球网络上转换您的节点订阅。

## 使用方式

- ### UI
  
  在 Cloudflare Pages 部署后打开，并按提示操作。

- ### Clash Meta

  端点：`/sub/clash-meta`
  
  需提供以下参数：
  
  - `url`: 订阅的远程地址
  - `remote_config` (可选): 远程配置地址 (INI 格式)，默认为 https://raw.githubusercontent.com/kobe-koto/EdgeSub/main/public/minimal_remote_rules.ini
  - `udp` (可选): 是否启用 UDP，默认为 true
  - `forced_refresh` (可选): 是否强制刷新已缓存的远程配置，默认为 false

- ### Debug

  > **⚠️ 警告**
  > 
  > 此格式可能随时变更，仅作测试用途
  
  端点：`/sub/debug`
  
  需提供以下参数：
  
  - `url`: 订阅的远程地址

## 部署

- ### 在 Cloudflare Pages 上部署

  0. Fork 本项目
  1. 打开 dash.cloudflare.com
  2. 转到侧边栏的 **Workers & Pages** (Overview) 部分
  3. 点击 **Create** 按钮
  4. 切换到 **Pages** 标签页
  5. 选择 **Connect to Git**
  6. 选择您 Fork 的 EdgeSub 项目
  7. 在 **Build settings > Framework preset** 中选择 Astro
  8. 编辑 **Build settings > Build command** 为 `pnpm build:frontend`
  9. 点击 **Save and Deploy**
  10. 部署完成后，可前往 **项目 > Custom domains** 添加自定义域名
  
  #### ✡️ 可选 - 为远程规则添加缓存加速：
  
  **注意：开发时默认缓存 KV 存在，无缓存 KV 环境将按低优先级开发。**
  
  **强烈建议添加缓存 KV。**
  
  1. 转到 **Workers & Pages > KV**
  2. 点击 **Create a namespace**，输入名称后点击 **Add**
  3. 返回项目
  4. 转到 **Settings > Functions > KV namespace bindings**
  5. 点击 **Add binding**
  6. **Variable name** 填写 `EdgeSubDB`，**KV namespace** 选择新建的 KV 空间
  7. 点击 **Save**
  8. 转到 **项目 > Deployments**
  9. 在 **All deployments** 中选择最新部署，点击 **右侧三点菜单 > Retry Deployment**
  10. 完成

## 兼容性表格

- 节点类型

  | 类型         | 支持 | 已测试 | 备注                                |
  | ------------ | ---- | ------ | ----------------------------------- |
  | HTTP         | ✅    | 🚫      |                                     |
  | Socks 5      | ✅    | 🚫      |                                     |
  | Hysteria 1   | ✅    | ✅      |                                     |
  | Hysteria 2   | ✅    | ✅      |                                     |
  | TUIC v5      | ✅    | ✅      |                                     |
  | Vmess        | ✅    | ☑️      | 未完全测试                          |
  | Vless        | ✅    | ☑️      | 未完全测试                          |
  | Shadowsocks  | ✅    | ✅      | 全局设置开启 SS UoT 时会启用 UDP over TCP |
  | Trojan       | ✅    | ✅      |                                     |
  | WireGuard    | 🚫    | -      | 无通用 ShareLink 格式               |
  | ShadowsocksR | 🚫    | -      | 暂无实现计划                        |
  | SSH          | 🚫    | -      | 暂无实现计划                        |

- 订阅类型

  | 类型                    | 输入 | 输出 | 输出端点          |
  | ----------------------- | ---- | ---- | ----------------- |
  | 内部调试格式            | 🚫    | ✅    | `/sub/debug`      |
  | ShareLink 集合          | ✅    | ✅    | `/sub/share-link` |
  | ShareLink 集合 (Base64) | ✅    | ✅    | `/sub/base64`     |
  | Clash Meta 配置         | ✅    | ✅    | `/sub/clash-meta` |
  | Clash 配置              | ✅    | ✅    | `/sub/clash`      |
  | Sing-Box 配置           | ✅    | ✅    | `/sub/sing-box`   |

  注：
  
  - **Clash Meta 和 Clash 配置**： 
    - 输入：暂不特殊处理
    - 输出：Clash 配置会过滤仅保留支持的代理类型，其余与 Clash Meta 相同
  
  - **内部调试格式**： 
    仅供调试，未来可能进行破坏性变更或移除。
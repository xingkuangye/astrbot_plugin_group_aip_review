# AstrBot 群聊内容安全审查插件

基于百度/阿里云内容审核API的群聊内容安全审查插件，支持文本、图片、GIF表情包内容审核，自动撤回违规内容并发送到通知群。让你的Bot化身消息审核小助手。

**v2.0.0 新增**：文本和图片审核API独立配置，支持同时启用多个API进行交叉审核，显著提升违规检测准确率。

## 功能特性

- **多API支持**：支持百度内容审核API和阿里云内容安全API，可同时启用多个API进行交叉审核
- **独立配置**：文本审核和图片审核可分别选择不同的API组合
- **交叉审核**：同时启用多个API时，任一API检测到违规即触发处理，大幅降低漏检率
- **文本内容审核**：实时检测群聊文本消息的合规性
- **图片内容审核**：自动审核群聊中的图片内容
- **智能违规处置**：根据审核结果自动撤回消息、禁言、踢人等
- **多层级配置**：支持全局默认配置和群组自定义配置
- **违规记录管理**：统计用户和群组的违规次数，智能触发惩罚机制
- **通知机制**：审核结果实时通知到指定群组

## 安装方法

### 方式一：插件市场安装
1. 在插件市场中搜索插件 `astrbot_plugin_group_aip_review` 或 `群消息内容安全审核插件`
2. 或将本仓库地址复制后，在插件管理页面输入链接安装

### 方式二：手动安装
1. 克隆本仓库到本地
2. 在 WebUI 的平台日志页面右上角安装依赖
3. 安装以下Python包：
   - `baidu-aip`（百度审核SDK）
   - `aliyunsdkcore` + `aliyunsdkgreen`（阿里云审核SDK）
   - `httpx`（HTTP客户端，用于图片下载）

## API配置

### 选择审核API

插件支持两种内容审核API，且文本和图片可以独立选择不同的API组合：

#### 文本审核API配置
通过 `text_api_providers` 配置项选择文本审核使用的API：
- `["baidu"]` - 仅使用百度（默认）
- `["aliyun"]` - 仅使用阿里云
- `["baidu", "aliyun"]` - 同时使用百度和阿里云（交叉审核）

#### 图片审核API配置
通过 `image_api_providers` 配置项选择图片审核使用的API：
- `["baidu"]` - 仅使用百度（默认）
- `["aliyun"]` - 仅使用阿里云
- `["baidu", "aliyun"]` - 同时使用百度和阿里云（交叉审核）

> [!TIP]
> 同时启用多个API进行交叉审核时，任一API检测到违规内容即触发处置措施，可显著降低漏检率。

### 百度API配置

在插件配置页面填写以下必填项：
- `api_key`：百度云API Key（从[百度云控制台](https://console.bce.baidu.com/)获取）
- `secret_key`：百度云Secret Key（从[百度云控制台](https://console.bce.baidu.com/)获取）
- `strategy_id`：自定义审核策略ID（可选）

### 阿里云API配置

在插件配置页面填写以下必填项：
- `access_key_id`：阿里云AccessKey ID（从[阿里云RAM控制台](https://ram.console.aliyun.com/)获取）
- `access_key_secret`：阿里云AccessKey Secret（从[阿里云RAM控制台](https://ram.console.aliyun.com/)获取）
- `region`：阿里云地域（默认 `cn-shanghai`，支持 `cn-beijing`、`cn-shenzhen`、`ap-southeast-1`）

> [!TIP]
> 阿里云内容安全提供免费试用额度，个人开发者可免费使用。访问[阿里云内容安全](https://www.aliyun.com/product/lvwang)了解更多。

## 审核处置配置

### 默认全局配置

- `notify_group_id`：默认通知群ID（所有无自定义的群共用）
- `admin_id`：管理员ID（审核失败时通知）
- `single_user_violation_threshold`：单人短时间违规次数阈值（默认3次，设置为0表示不启用单人禁言功能）
- `group_violation_threshold`：群内短时间违规次数阈值（默认5次，设置为0表示不启用全员禁言功能）
- `time_window`：统计时间窗口（秒，默认300秒=5分钟）
- `mute_duration`：单人禁言时长（秒，默认86400秒=1天）
- `kick_user`：是否启用踢人（默认false）
- `kick_user_threshold`：踢人阈值（默认5次，设置为0表示不启用踢人功能）
- `is_kick_user_and_block`：是否踢出并拉黑用户（默认false）

### 群组自定义配置

支持为每个群组单独配置，格式为：

```json
{
  "群ID": {
    "rule_id": "规则标识",
    "notify_group_id": "通知群ID",
    "single_user_violation_threshold": 2,
    "group_violation_threshold": 4,
    "time_window": 600,
    "mute_duration": 43200
  }
}
```

## 功能开关

- `enabled_groups`：启用插件的群号列表（默认空列表，不对任何群生效）
- `enable_text_censor`：是否启用文本审核（默认true）
- `enable_image_censor`：是否启用图片审核（默认true）
- `log_level`：日志级别（默认INFO）

## 命令

- `/开启内容审核` - 开启当前群的内容审核（需要管理员权限）
- `/关闭内容审核` - 关闭当前群的内容审核（需要管理员权限）
- `/查看审核配置` - 查看当前群的审核配置（需要管理员权限）

## 技术支持
如有问题或建议，请通过以下方式联系：
- 插件作者：[@VanillaNahida](https://github.com/VanillaNahida)
- GitHub仓库：https://github.com/xingkuangye/astrbot_plugin_group_aip_review
- QQ群：[195260107](https://qm.qq.com/q/1od5TMYrKE)

## 更新日志

### v2.0.0
- 新增文本审核API和图片审核API独立配置功能
- 支持同时启用多个API进行交叉审核
- 交叉审核模式下，任一API检测违规即触发处置
- 显著提升违规内容检测的准确率和覆盖率

### v1.1.0
- 新增阿里云内容安全API支持
- 支持在百度API和阿里云API之间自由切换
- 保持向后兼容性，原有百度API配置继续有效
- 阿里云支持多场景检测：色情、暴恐、广告、二维码等
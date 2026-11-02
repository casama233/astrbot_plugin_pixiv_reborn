# AstrBot Pixiv 搜尋插件

[简体中文](README.md) · **繁體中文** · [English](README.en.md)

使用 AstrBot 搜尋 Pixiv 插畫、小說、排行榜、推薦、動圖（ugoira）及 Fanbox 公開／授權內容。支援不同搜尋模式、內容過濾、訂閱、自動圖片發送及管理功能。

## 主要功能與指令

| 類別 | 指令 |
| --- | --- |
| 標籤搜尋 | `/pixiv <標籤>`、`/pixiv_deepsearch <標籤>`、`/pixiv_and <標籤>` |
| 使用者／作品 | `/pixiv_user_search <名稱>`、`/pixiv_user_detail <ID>`、`/pixiv_specific <作品ID>`、`/pixiv_related <作品ID>` |
| 小說 | `/pixiv_novel <標籤>`、`/pixiv_novel download <ID>`、`/pixiv_novel_new`、`/pixiv_novel_recommended`、`/pixiv_novel_series <ID>` |
| 推薦／排行榜 | `/pixiv_recommended`、`/pixiv_ranking [模式] [日期]`、`/pixiv_trending_tags`、`/pixiv_illust_new` |
| 熱門標籤 | `/pixiv_hot <標籤> [時間範圍] [頁數]` |
| 評論／特輯 | `/pixiv_illust_comments <ID>`、`/pixiv_novel_comments <ID>`、`/pixiv_showcase_article <ID>` |
| 訂閱 | `/pixiv_subscribe_add <畫師ID>`、`/pixiv_subscribe_remove <畫師ID>`、`/pixiv_subscribe_list` |
| AI 作品 | `/pixiv_ai_show_settings <設定>` |
| 隨機搜尋 | `/pixiv_random_add`、`_list`、`_del`、`_suspend`、`_resume`、`_status`、`_force`（完整前綴為 `/pixiv_random_...`） |
| 隨機排行榜 | `/pixiv_random_ranking_add`、`_del`、`_list` |
| Fanbox | `/pixiv_fanbox_creator`、`_post`、`_recommended`、`_artist`、`_dl`、`_dl_status`、`_dl_stop`、`_dl_view` |
| 配置 | `/pixiv_config show`、`/pixiv_config help` 等 |

以 `-標籤` 排除負面標籤，例如 `/pixiv 初音ミク,-ntr`；隨機搜尋也支援相同語法。Fanbox 批量下載支援 `--limit`、`--since`、`--type image|file|all`、`--dir`、`--force`，可用 `--pack` 取得加密／分卷壓縮檔，並可查看狀態或停止。所有指令保持原有字串，**README 翻譯不等於命令被本地化**。

## 安裝與認證

需要 **Python 3.10+、AstrBot v3.x+、有效 Pixiv 帳戶及 `refresh_token`**。於 AstrBot WebUI → 插件管理 → Pixiv 搜尋 → 插件設定，填寫 Refresh Token；需要存取 Fanbox 限制內容時，另設定 `FANBOXSESSID`（`fanbox_sessid`）。

取得 token 可參考 [pixivpy3](https://pypi.org/project/pixivpy3/) 及 [Pixiv OAuth 教學](https://gist.github.com/ZipFile/c9ebedb224406f4f11845ab700124362)。**請勿把 Refresh Token、Session Cookie、cf_clearance 或代理憑證貼到公開 Issue／日誌。**

Fanbox 資料來源：`auto`（預設，官方優先、失敗退回 Nekohouse）、`official` 或 `nekohouse`。可配置 R18／R18G 過濾、AI 作品顯示、最低收藏／瀏覽／按讚數、圖片品質和回傳數量。

## 基本配置

| 欄位 | 預設／用途 |
| --- | --- |
| `refresh_token` | Pixiv API 認證，必填 |
| `fanbox_sessid` | 可選的 Fanbox Session |
| `fanbox_data_source` | `auto`／`official`／`nekohouse`，預設 `auto` |
| `fanbox_cf_clearance`、`fanbox_user_agent` | 官方 Cloudflare 阻擋時選用 |
| `fanbox_dl_impersonate` | 批量下載 TLS 模擬指紋，如 `edge101`、`chrome131` |
| `fanbox_api_proxy_host` | Fanbox API 自訂代理主機 |
| `fanbox_dl_*` | 批量下載頁數、429 重試、CF 失敗上限、併發、佇列、100MB 分卷等 |
| `return_count` | 1–10，預設 1 |
| `r18_mode` | 預設過濾 R18，另有僅 R18／允許模式 |
| `filter_r18g_only` | 預設 `false` |
| `ai_filter_mode` | AI 作品顯示策略 |
| `ai_detection_mode` | `field_or_tag`／`field_only`／`tag_only`，預設第一項 |
| `min_bookmarks`／`min_views`／`min_likes` | 品質門檻，預設 0 |
| `deep_search_depth` | 預設 3；`-1` 無限制、0–50 |
| `single_response_mode` | 預設 `true`，整合為單則回覆 |
| `image_send_method` | `url`／`file`／`byte`；升級後可考慮後兩者 |
| `image_quality` | `original`／`large`／`medium`，預設 `medium` |
| `pil_compress_quality`／`pil_compress_target_kb` | 本地檔案／位元組圖片壓縮 |
| `subscription_enabled` | 預設 `true` |
| `subscription_check_interval_minutes` | 預設 30 分鐘 |
| `refresh_token_interval_minutes` | 預設 180 分鐘 |
| `proxy`、`image_proxy_host`、`use_image_proxy` | 網路及圖片代理；圖片代理預設 `i.pixiv.re` |
| `random_search_min_interval`／`random_search_max_interval` | 隨機搜尋預設 60／120 分鐘 |

完整欄位、壓縮、圖文合併、訂閱與歷史更新內容見[簡中完整版](README.md)。

## 網路與 Fanbox 排障

SSL 錯誤先檢查 DNS；認證失敗請檢查 Refresh Token；缺少模組可嘗試重新啟動 AstrBot。

無法直接存取 Pixiv API 時，可以自建 [pixiv-proxy Cloudflare Worker](https://github.com/vmoranv/pixiv-proxy) 並設定 `api_proxy_host` 或 `image_proxy_host`。Fanbox 403 可能是權限或 Cloudflare 阻擋；先測試公開貼文，必要時設定合法取得的 Session，或選擇可用的資料來源。批量下載有時需讓 `fanbox_dl_impersonate` 與 `fanbox_user_agent` 匹配瀏覽器類別，必要時再用相應 `fanbox_cf_clearance`；不能保證繞過所有存取限制。

## 授權與使用邊界

遵守 Pixiv 服務條款、創作者權利及所在地法律，避免過量 API 請求。本專案是 [vmoranv-reborn/astrbot_plugin_pixiv_reborn](https://github.com/vmoranv-reborn/astrbot_plugin_pixiv_reborn) 的 fork；上游許可 **AGPL-3.0**，須保留其原作者聲明和相應授權。本 fork 不宣稱重新擁有上游作品，第三方圖片、文案與資料依各自權利處理。

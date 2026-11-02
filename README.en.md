# AstrBot Pixiv Search Plugin

[简体中文](README.md) · [繁體中文](README.zh-TW.md) · **English**

An AstrBot plugin for searching Pixiv illustrations, novels, rankings, recommendations, ugoira animations and public/authorized Fanbox content. Includes search filters, subscriptions, automatic image replies and administrative settings.

## Key commands

| Group | Commands |
| --- | --- |
| Tag search | `/pixiv <tags>`, `/pixiv_deepsearch <tags>`, `/pixiv_and <tags>` |
| User / work | `/pixiv_user_search <name>`, `/pixiv_user_detail <ID>`, `/pixiv_specific <workID>`, `/pixiv_related <workID>` |
| Novels | `/pixiv_novel <tags>`, `/pixiv_novel download <ID>`, `/pixiv_novel_new`, `/pixiv_novel_recommended`, `/pixiv_novel_series <ID>` |
| Discovery | `/pixiv_recommended`, `/pixiv_ranking [mode] [date]`, `/pixiv_trending_tags`, `/pixiv_illust_new` |
| Popular works | `/pixiv_hot <tags> [period] [page_count]` |
| Comments / showcases | `/pixiv_illust_comments <ID>`, `/pixiv_novel_comments <ID>`, `/pixiv_showcase_article <ID>` |
| Subscriptions | `/pixiv_subscribe_add <artistID>`, `/pixiv_subscribe_remove <artistID>`, `/pixiv_subscribe_list` |
| AI content | `/pixiv_ai_show_settings <setting>` |
| Random searches | `/pixiv_random_add`, `/pixiv_random_list`, `/pixiv_random_del`, `/pixiv_random_suspend`, `/pixiv_random_resume`, `/pixiv_random_status`, `/pixiv_random_force` |
| Random rankings | `/pixiv_random_ranking_add`, `/pixiv_random_ranking_del`, `/pixiv_random_ranking_list` |
| Fanbox | `/pixiv_fanbox_creator`, `/pixiv_fanbox_post`, `/pixiv_fanbox_recommended`, `/pixiv_fanbox_artist`, `/pixiv_fanbox_dl`, `/pixiv_fanbox_dl_status`, `/pixiv_fanbox_dl_stop`, `/pixiv_fanbox_dl_view` |
| Configuration | `/pixiv_config show`, `/pixiv_config help`, and related settings |

Prefix a tag with `-` to exclude it, e.g. `/pixiv 初音ミク,-ntr`. Negative tags also work with random searches. Fanbox bulk download supports `--limit`, `--since`, `--type image|file|all`, `--dir` and `--force`. `--pack` can return an encrypted/split archive. **Translating the README does not translate or rename actual commands.**

## Requirements, setup and authentication

Requires **Python 3.10+**, an AstrBot **v3.x+** installation, a valid Pixiv account and its **`refresh_token`**. Open AstrBot WebUI → Plugin manager → Pixiv Search → Settings and enter the token. For authorized restricted Fanbox access, optionally set `fanbox_sessid` (`FANBOXSESSID`).

See [pixivpy3](https://pypi.org/project/pixivpy3/) and [Pixiv OAuth instructions](https://gist.github.com/ZipFile/c9ebedb224406f4f11845ab700124362) for token guidance. **Never post tokens, Fanbox session cookies, cf_clearance or proxy credentials in public issues or logs.**

Fanbox sources: `auto` (default, official first with Nekohouse fallback), `official` or `nekohouse`. Configure R18/R18G filtering, AI-content display, minimum bookmarks/views/likes, image quality and result count.

## Main settings

| Setting | Purpose / default |
| --- | --- |
| `refresh_token` | Pixiv API token, required |
| `fanbox_sessid` | Optional Fanbox session |
| `fanbox_data_source` | `auto` / `official` / `nekohouse`; default `auto` |
| `fanbox_cf_clearance`, `fanbox_user_agent` | Optional for official Cloudflare failures |
| `fanbox_dl_impersonate` | Bulk-download TLS fingerprint, e.g. `edge101` or `chrome131` |
| `fanbox_api_proxy_host` | Optional Fanbox API proxy host |
| `fanbox_dl_*` | Bulk download paging, 429 retries, CF errors, concurrency/queue, 100 MB archive parts |
| `return_count` | 1–10 images, default 1 |
| `r18_mode` | Default filter R18; allow-only or allow variants supported |
| `filter_r18g_only` | Default `false` |
| `ai_filter_mode` | AI-work display mode |
| `ai_detection_mode` | `field_or_tag` / `field_only` / `tag_only`, default first |
| `min_bookmarks`, `min_views`, `min_likes` | Minimum quality criteria, default 0 |
| `deep_search_depth` | Default 3; `-1` unlimited, or 0–50 |
| `single_response_mode` | Single merged response, default `true` |
| `image_send_method` | `url` / `file` / `byte` |
| `image_quality` | `original` / `large` / `medium`; default `medium` |
| `pil_compress_quality`, `pil_compress_target_kb` | File/byte image compression |
| `subscription_enabled` | Default `true` |
| `subscription_check_interval_minutes` | Default 30 |
| `refresh_token_interval_minutes` | Default 180 |
| `proxy`, `image_proxy_host`, `use_image_proxy` | Network/image proxy; default image host `i.pixiv.re` |
| `random_search_min_interval`, `random_search_max_interval` | Default 60 / 120 minutes |

For exhaustive parameters, examples and historical changelog, see the [canonical Chinese documentation](README.md).

## Networking and Fanbox troubleshooting

For SSL errors, check DNS/network configuration. For authentication failures, validate the Refresh Token. Restart AstrBot if a newly installed module is not found.

When Pixiv API access is unavailable, you may deploy your own [pixiv-proxy Cloudflare Worker](https://github.com/vmoranv/pixiv-proxy) and configure `api_proxy_host` or `image_proxy_host`. Fanbox 403s can reflect access permissions or Cloudflare defenses. First test public posts, then configure a legitimately obtained session if needed, or select a supported data source. Bulk downloads may require `fanbox_dl_impersonate` and `fanbox_user_agent` to use the same browser family, possibly with matching `fanbox_cf_clearance`. No access method is guaranteed to work for all protected content.

## License and acceptable use

Follow Pixiv's terms, creator rights and applicable law; avoid excessive API requests. This repository forks [vmoranv-reborn/astrbot_plugin_pixiv_reborn](https://github.com/vmoranv-reborn/astrbot_plugin_pixiv_reborn) under **AGPL-3.0**. Preserve upstream licensing and attribution; the fork does not claim original authorship or relicense third-party artwork, descriptions or data.

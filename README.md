# OpenSERP (Search Engine Results)

![OpenSERP](/logo.svg)

[![Go Report Card](https://goreportcard.com/badge/github.com/karust/openserp)](https://goreportcard.com/report/github.com/karust/openserp)
[![Go Reference](https://pkg.go.dev/badge/github/karust/openserp?style=for-the-badge)](https://pkg.go.dev/github.com/karust/openserp)
[![release](https://img.shields.io/github/release/karust/openserp)](https://github.com/karust/openserp/releases)

<!--[![Docker Pulls](https://img.shields.io/docker/pulls/karust/openserp)](https://hub.docker.com/repository/docker/karust/openserp)-->

**OpenSERP** provides free API and CLI access to multiple search engines including **[Google, Yandex, Baidu, Bing, DuckDuckGo]**. Get comprehensive search results without expensive API subscriptions!

## Features

- 🔍 **Multi-Engine Support**: Google, Yandex, Baidu, Bing, DuckDuckGo...
- 🌐 **Megasearch**: Aggregate results from multiple engines simultaneously
- 🖼 **Images**: Image search is also available!
- 🎯 **Advanced Filtering**: Language, date range, file type, site-specific searches
- 🌍 **Proxy Support**: HTTP/SOCKS5 proxy support
- 🐳 **Docker Ready**: Easy deployment with Docker

## Quick Start⚡️

### Docker (Recommended)

```bash
# Run the API server via prebuilt image
docker run -p 127.0.0.1:7000:7000 -it karust/openserp serve -a 0.0.0.0 -p 7000

# Or use docker-compose
docker compose up --build
```

### From Source

```bash
# Clone and build
git clone https://github.com/karust/openserp.git
cd openserp
go build -o openserp .

# Run the server
./openserp serve
```

## 🌐 Megasearch & Megaimage - Search Everything at Once!

**Megasearch** aggregates results from multiple engines simultaneously with automatic deduplication. **Megaimage** does the same for image searches!

### Megasearch (Web Results)

```bash
# Search ALL engines at once
curl "http://localhost:7000/mega/search?text=golang&limit=10"

# Pick specific engines
curl "http://localhost:7000/mega/search?text=golang&engines=duckduckgo,bing&limit=15"

# Advanced filtering
curl "http://localhost:7000/mega/search?text=Donald+Trump&engines=duckduckgo,bing&limit=20&date=20251005..20251005&lang=EN"
```

- API response example:

```json
[
  {
    "rank": 1,
    "url": "https://en.wikipedia.org/wiki/Golden_Retriever",
    "title": "Golden Retriever - Wikipedia",
    "description": "The Golden Retriever is a Scottish breed of retriever dog of medium size. It is characterised by a gentle and affectionate nature and a striking golden coat. It is a working dog, and registration is subject to successful completion of a working trial. [2] It is commonly kept as a companion dog and is among the most frequently registered breeds in several Western countries; some may compete in ...",
    "ad": false,
    "engine": "duckduckgo"
  },
  {
    "rank": 2,
    "url": "https://www.bing.com/ck/a?!&&p=6f15ac4589858d0a104cd6f55cc8e91e8d8d6da91f905b626921f67f2323a467JmltdHM9MTc1OTE5MDQwMA&ptn=3&ver=2&hsh=4&fclid=2357c2f4-6131-68de-359f-d48c607c691d&u=a1aHR0cHM6Ly93d3cuZ29sZGVucmV0cmlldmVyZm9ydW0uY29tL3RocmVhZHMvdW5kZXJzdGFuZGluZy13aHktZ29sZGVuLXJldHJpZXZlciVFMiU4MCU5OXMtbGlmZXNwYW4taGFsdmVkLWluLXRoZS1sYXN0LTM1LXllYXJzLjM1NzMyMi8&ntb=1",
    "title": "Golden Retriever Dog Forums\nhttps://www.goldenretrieverforum.com › threads › understanding-why-g...",
    "description": "Oct 20, 2024 · Back in the 1970s, Golden Retrievers routinely lived until 16 and 17 years old, they are now living until 9 or 10 years old. Golden Retrievers seem to be dying mostly of bone ...",
    "ad": false,
    "engine": "bing"
  },
  {
    "rank": 3,
    "url": "http://www.baidu.com/link?url=2544q3ugc68j0scVxdpWCSX-gl2AmuCy1l7uRR3loIfS1hmJWMiJKW4MDGWoZrLE7X-ybu1L7T8PspoL7iy_dK",
    "title": "golden retrievers是什么意思_golden retrievers怎么读_解释_用法...",
    "description": "\n\n2025年9月21日golden retrievers 读音:美英 golden retrievers基本解释 金毛猎犬 分词解释 golden金(黄)色的 retrievers寻猎物犬( retriever的名词复数 ) 词组短语 golden retrieversfor sale出售金毛寻回犬 golden retrieversnear me我附近的金毛寻回犬 golden retrieverspuppies金毛寻回犬幼犬...\ndanci.gei6.com/golden...retrievers...",
    "ad": false,
    "engine": "baidu"
  }
]
```

### Megaimage (Image Results)

```bash
# Search images across ALL engines
curl "http://localhost:7000/mega/image?text=golang logo&limit=20"
```

### Available Engines

```bash
# Check which engines are available
curl "http://localhost:7000/mega/engines"
```

**Available engines:** `google`, `yandex`, `baidu`, `bing`, `duckduckgo`

## 🔍 Individual Engine APIs

### Search Parameters

| Parameter | Description          | Example                           |
| --------- | -------------------- | --------------------------------- |
| `text`    | Search query         | `golang programming`              |
| `lang`    | Language code        | `EN`, `DE`, `RU`, `ES`            |
| `date`    | Date range           | `20230101..20231231`              |
| `file`    | File extension       | `PDF`, `DOC`, `XLS`               |
| `site`    | Site-specific search | `github.com`, `stackoverflow.com` |
| `limit`   | Number of results    | `10`, `25`, `50`                  |

### Engine-Specific Parameters

| Parameter | Supported engines                   | Notes                                                                  |
| --------- | ----------------------------------- | ---------------------------------------------------------------------- |
| `start`   | `google`, `bing`, `yandex`, `baidu` | Web search pagination offset.                                          |
| `filter`  | `google`                            | Duplicate filter (`true` => hide similar, `false` => include similar). |
| `answers` | `google`                            | Include Google answer boxes in output (negative ranks).                |

### Individual Engine Examples

```bash
# DuckDuckGo search
curl "http://localhost:7000/duck/search?text=golang&limit=7"

# Google search
curl "http://localhost:7000/google/search?text=golang&lang=EN&limit=10"

# Google search (pagination + include similar/hidden results)
curl "http://localhost:7000/google/search?text=golang&lang=EN&limit=10&start=10&filter=false"

# Bing search (offset 20 => around page 3 for 10 results/page)
curl "http://localhost:7000/bing/search?text=golang&limit=10&start=20"

# Yandex search (offset 10 => second page)
curl "http://localhost:7000/yandex/search?text=golang&limit=10&start=10"
```

### Image Search

```bash
# Bing Images
curl "http://localhost:7000/bing/image?text=golang&limit=20"

# Baidu Images
curl "http://localhost:7000/baidu/image?text=golang&limit=15"
```

## 🌐 Proxy Support

OpenSERP supports HTTP and SOCKS5 proxies with authentication:

```bash
# SOCKS5 proxy
./openserp serve --proxy socks5://127.0.0.1:1080

# HTTP proxy with authentication
./openserp search bing "query" --proxy http://user:pass@127.0.0.1:8080
```

## Health Check

```bash
curl -i "http://127.0.0.1:7000/health"
```

Response includes:

- `status`: `healthy`, `degraded`, or `unhealthy`
- `uptime`
- per-engine readiness in `engines`
- basic runtime stats in `system`

HTTP status behavior:

- `200` for `healthy` and `degraded`
- `503` for `unhealthy`

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 👾 Issues & Support

If you encounter any issues or have questions:

- Open an issue on GitHub
- Check existing issues for solutions
- Review the documentation above

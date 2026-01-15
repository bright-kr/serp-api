# SERP API

[![Promo](https://media.brightdata.com/2025/08/SERP-API-50-off-GitHub-banner_1389_166.png)](https://brightdata.co.kr/products/serp-api) 

이 리포지토리는 검색 엔진 결과 페이지(SERP) 데이터를 수집하기 위한 두 가지 접근 방식을 제공합니다:
1. 기본적인 데이터 수집에 적합한 무료 소규모 Google スクレイパー
2. 주요 검색 엔진에서 대규모 실시간 데이터 수집을 위한 엔터프라이즈급 API 솔루션

## Table of Contents
- [Free SERP Scraper](#free-serp-scraper)
   - [Input Parameters](#input-parameters)
   - [Implementation](#implementation)
   - [Sample Output](#sample-output)
- [Limitations](#limitations)
- [Bright Data SERP API](#bright-data-serp-api)
   - [Key Features](#key-features)
   - [Getting Started](#getting-started)
   - [Direct API Access](#direct-api-access)
   - [Native Proxy-Based Access](#native-proxy-based-access)
- [Query Parameters Overview](#query-parameters-overview)
   - [Google](#google)
     - [Google Search](#1-google-search)
     - [Google Maps](#2-google-maps)
     - [Google Trends](#3-google-trends)
     - [Google Reviews](#4-google-reviews)
     - [Google Lens](#5-google-lens)
     - [Google Hotels](#6-google-hotels)
     - [Google Flights](#7-google-flights)
   - [Bing](#bing)
   - [Yandex](#yandex)
   - [DuckDuckGo](#duckduckgo)
- [Other Settings for SERP API](#other-settings-for-serp-api)
   - [Asynchronous Requests](#asynchronous-requests)
   - [Multi-Query Requests](#multi-query-requests)
- [Support & Resources](#support--resources)

## Free SERP Scraper
[무료 スクレイパー](https://github.com/bright-kr/serp-api/tree/main/free_serp_scraper)를 사용하면 소규모 Google SERP 데이터 수집이 가능합니다.

<img width="700" alt="google-search" src="https://github.com/bright-kr/serp-api/blob/main/Images/bright%20data%20products%20serp.png" />


### Input Parameters
- **File:** 검색어를 포함한 텍스트 파일(필수)
- **Format:** 한 줄당 검색어 1개

### Implementation
Python 파일에서 다음 파라미터를 수정합니다:
```python
# free_serp_scraper/google_serp.py
HEADLESS = False
MAX_RETRIES = 2
REQUEST_DELAY = (1, 4)

with open("search_terms.txt", "r", encoding="utf-8") as file:
    pass
```

### Sample Output
<img width="700" alt="google-serp-data" src="https://github.com/bright-kr/serp-api/blob/main/Images/samle%20output%20serp.png" />


## Limitations
Google은 여러 가지 스クレイピング 방지 조치를 구현합니다:

1. **CAPTCHA**: 사람과 봇을 구분하기 위해 사용됩니다
2. **IP 차단**: 의심스러운 활동에 대해 일시적 또는 영구적으로 차단합니다
3. **속도 제한**: 식별되지 않은 요청를 신속히 탐지하고 차단합니다
4. **지오로케이션 타기팅**: 위치, 언어, 디바이스에 따라 결과가 달라집니다
5. **허니팟 트랩**: 자동화된 접근을 탐지하기 위한 숨겨진 요소입니다

## Bright Data SERP API
[Bright Data의 SERP API](https://brightdata.co.kr/products/serp-api)는 신뢰할 수 있는 SERP 데이터 수집을 위한 견고한 솔루션을 제공합니다.

### Key Features

- 성공한 요청당 과금 모델
- 빠른 응답 시간
- 위치 기반 타기팅
- 다양한 디바이스 유형 및 검색 매개변수 지원
- 주요 검색 엔진 커버리지(Google, Bing, DuckDuckGo, Yandex, Baidu, Yahoo, Naver)
- 내장 안티봇 솔루션
- 도시 단위 정확도를 갖춘 실시간 결과
- 구조화된 데이터 출력(JSON/HTML)

**Note:** **SERP API**는 [**Bright Data’s Web Scraping Suite**](https://docs.brightdata.com/scraping-automation/introduction)의 일부이며, 완전한 프록시 관리, 언블로킹 및 파싱 기능을 포함합니다.

### Getting Started

1. **Prerequisites:**
    - [Bright Data 계정](https://brightdata.co.kr/)을 생성합니다
    - [API key](https://docs.brightdata.com/general/account/api-token)를 발급받습니다
2. **Setting Up SERP API:** [단계별 가이드](https://github.com/bright-kr/SERP-API/blob/main/setup_serp_api.md)를 따라 Bright Data 계정에서 새 SERP API를 설정합니다.
3. **Implementation Methods:**
    1. Direct API Access
    2. Native Proxy-Based Access

### Direct API Access
API를 사용하는 가장 간단한 방법은 직접 요청를 보내는 것입니다.

**cURL Example**
```bash
curl https://api.brightdata.com/request \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer API_TOKEN" \
  -d '{
        "zone": "ZONE_NAME",
        "url": "https://www.google.com/search?q=ollama&brd_json=1",
        "format": "raw"
      }'
```

**Python Example**
```python
import requests
import json

url = "https://api.brightdata.com/request"

headers = {"Content-Type": "application/json", "Authorization": "Bearer API_TOKEN"}

payload = {
    "zone": "ZONE_NAME",
    "url": "https://www.google.com/search?q=ollama&brd_json=1",
    "format": "raw",
}

response = requests.post(url, headers=headers, json=payload)

with open("serp_direct_api.json", "w") as file:
    json.dump(response.json(), file, indent=4)

print("Response saved to 'serp_direct_api.json'.")
```

👉 [전체 JSON 출력](https://github.com/bright-kr/SERP-API/blob/main/serp_api_outputs/serp_direct_api.json)을 확인하십시오

**Note**: 파싱된 JSON을 받으려면 `brd_json=1`을 사용하고, 파싱된 JSON + 전체 중첩 HTML을 받으려면 `brd_json=html`을 사용하십시오.

파싱 결과에 대해 더 알아보기: [SERP API Parsing Guide](https://docs.brightdata.com/scraping-automation/serp-api/parsing-search-results)

### Native Proxy-Based Access
프록시 라우팅을 사용하는 대체 방법입니다.

**cURL Example**
```bash
curl -i \
  --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<CUSTOMER_ID>-zone-<ZONE_NAME>:<ZONE_PASSWORD> \
  -k \
  "https://www.google.com/search?q=ollama"
```

**Python Example**
```python
import requests
import urllib3

urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

host = "brd.superproxy.io"
port = 33335
username = "brd-customer-<customer_id>-zone-<zone_name>"
password = "<zone_password>"
proxy_url = f"http://{username}:{password}@{host}:{port}"

proxies = {"http": proxy_url, "https": proxy_url}

url = "https://www.google.com/search?q=ollama"
response = requests.get(url, proxies=proxies, verify=False)

with open("serp_native_proxy.html", "w", encoding="utf-8") as file:
    file.write(response.text)

print("Response saved to 'serp_native_proxy.html'.")
```

👉 [전체 HTML 출력](https://github.com/bright-kr/SERP-API/blob/main/serp_api_outputs/serp_native_proxy.html)을 확인하십시오

**SSL Certificate**: 프로덕션에서는 Bright Data의 SSL 인증서를 로드하십시오. 자세히 알아보기: [SSL Certificate Guide](https://docs.brightdata.com/general/account/ssl-certificate)

## Query Parameters Overview
Bright Data SERP API를 사용하면 로컬라이제이션, 페이지네이션, 디바이스 에뮬레이션 등과 같은 쿼리 매개변수를 통해 Google, Bing, Yandex, DuckDuckGo를 포함한 여러 검색 엔진에 대한 요청를 커스터마이즈할 수 있습니다. 이 개요는 API 기능을 높은 수준에서 보여드립니다.

> 모든 쿼리 매개변수의 전체 목록과 상세 설명은 [Detailed Query Parameters Documentation](https://docs.brightdata.com/scraping-automation/serp-api/query-parameters)을 참조하십시오.

### Google
SERP API는 [**Search**](https://brightdata.co.kr/products/serp-api/google-search), [**Maps**](https://brightdata.co.kr/products/serp-api/google-search/maps), [**Trends**](https://brightdata.co.kr/products/serp-api/google-search/trends), [**Reviews**](https://brightdata.co.kr/products/serp-api/google-search/reviews), **Lens**, [**Hotels**](https://brightdata.co.kr/products/serp-api/google-search/hotels), [**Flights**](https://brightdata.co.kr/products/web-scraper/google-flights) 등 다양한 Google 서비스를 지원합니다. 아래는 각 서비스별 주요 구성 매개변수입니다:

#### 1. Google Search
로컬라이제이션, 검색 유형, 페이지네이션, 지오로케이션, 디바이스 타기팅 옵션으로 검색 결과를 커스터마이즈합니다.

**Localization**
- `gl`: 검색 위치의 국가 코드(예: `gl=us`).
- `hl`: 결과의 언어 코드(예: `hl=en`).

```bash
curl \
  --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
  "https://www.google.com/search?q=pizza&gl=us&hl=en"
```

**Search Type:**
검색 유형을 지정하려면 **`tbm`** 매개변수를 사용하십시오:

- Images: `tbm=isch`
- Shopping: `tbm=shop`
- News: `tbm=nws`
- Videos: `tbm=vid`

```bash
curl \
  --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
  "https://www.google.com/search?q=pizza&tbm=shop"
```

**Pagination:**
- `start`: 결과 오프셋(첫 페이지는 0, 두 번째는 20 등).
- `num`: 페이지당 결과 수(기본값 20).

```bash
curl \
  --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
  "https://www.google.com/search?q=pizza&start=20&num=50"
```

**Geolocation:**
- `uule`: 지리적으로 특정된 결과를 위한 인코딩된 위치 문자열

```bash
curl \
  --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
  "https://www.google.com/search?q=pizza&uule=w+CAIQICINVW5pdGVkK1N0YXRlcw"
```

**Device Targeting:**
**`brd_mobile`** 매개변수를 사용하십시오:

- `0`: 데스크톱(기본값)
- `1`: 랜덤 모바일
- 특정 값: `ios`(또는 `iphone`), `ipad`, `android`, `android_tablet`

```bash
curl \
  --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
  "https://www.google.com/search?q=pizza&brd_mobile=1"
```

#### 2. Google Maps
좌표를 지정하고 숙소 유형으로 필터링하여 지도 쿼리를 커스터마이즈합니다.

**Coordinates:**
- 형식: `@latitude,longitude,zoom`(예: zoom은 `3z`부터 `21z`까지).

```bash
curl \
  --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
  "https://www.google.com/maps/search/restaurants/@47.30227,1.67458,14.00z"
```

**Accommodation Search:**

- `brd_accomodation_type`:
    - `hotels`(기본값)
    - `vacation_rentals`

```bash
curl \
  --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
  "https://www.google.com/maps/search/hotels+new+york/?brd_accomodation_type=vacation_rentals"
```

#### 3. Google Trends
커스터마이즈 가능한 기간 및 위젯 옵션으로 트렌드 데이터를 조회합니다.

**Required Parameters:**

- `brd_json=1`: 파싱된 JSON 결과를 반환합니다.
- `brd_trends`: 위젯을 지정합니다(예: `timeseries,geo_map`).

**Time Range:**

- `date`: 기간을 정의합니다(예: 지난 하루는 `now 1-d`).

```bash
curl \
  --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
  "https://trends.google.com/trends/explore?q=pizza&date=now+1-d&brd_trends=timeseries,geo_map&brd_json=1"
```

#### 4. Google Reviews
기능 ID를 사용해 리뷰를 가져오고 필요에 따라 정렬합니다.

**Key Parameters:**

- `fid`: 검색 결과에서 가져온 Feature ID입니다.
- `sort`: 정렬 순서(예: `newestFirst`, `ratingHigh`).

```bash
curl \
  --proxy brd.superproxy.io:33335 \
  --proxy-user "brd-customer-<id>-zone-<name>:<pass>" \
  "https://www.google.com/reviews?fid=0x808fba02425dad8f&sort=newestFirst"
```

#### 5. Google Lens
URL 또는 파일 업로드로 이미지를 기반으로 검색합니다.

**Image Search:**

- `url`: 검색할 이미지 URL입니다.
- `brd_json=1`: 결과를 JSON으로 반환합니다.

```bash
curl \
  --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
  "https://lens.google.com/uploadbyurl?url=https://example.com/image.jpg&brd_json=1"
```

#### 6. Google Hotels
예약 날짜 및 통화 옵션으로 호텔 검색을 커스터마이즈합니다.

**Booking Parameters:**

- `brd_dates`: 체크인 및 체크아웃 날짜(`YYYY-MM-DD,YYYY-MM-DD`).
- `brd_currency`: 통화 코드(예: `USD`, `EUR`).

```bash
curl \
  --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
  "https://www.google.com/travel/hotels?q=hotels+new+york&brd_dates=2022-01-20,2022-02-05"
```

#### 7. Google Flights
유사한 로컬라이제이션 매개변수로 항공편을 검색합니다.

```bash
curl \
  --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
  "https://www.google.com/travel/flights?q=flights+new+york&gl=us&hl=en"
```


### Bing
로컬라이제이션, 지리 타기팅, 페이지네이션, 디바이스 및 브라우저 타기팅, 출력 형식 옵션으로 Bing 쿼리를 구성합니다. [전용 Bing API](https://brightdata.co.kr/products/serp-api/bing-search)도 확인하십시오.

**Localization**

- `setLang`: 인터페이스 언어(예: `setLang=en-US`).

```bash
curl \
  --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
  "https://www.bing.com/search?q=pizza&setLang=en-US"
```

**Geo-Location**

- `location`: 검색 출발지(예: `location=New+York`).
- `cc`: 국가 코드(예: `cc=us`).
- `mkt`: 마켓 코드(예: `mkt=en-US`).


```bash
curl \
  --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
  "https://www.bing.com/search?q=pizza&location=New+York&cc=us&mkt=en-US"
```

**Pagination**

- `count`: 결과 수(예: `count=50`).
- `first`: 페이지네이션 오프셋(예: 두 번째 페이지는 `first=11`).


```bash
curl \
  --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
  "https://www.bing.com/search?q=pizza&count=50&first=11"
```

**Filters**

- `safesearch`: 성인 콘텐츠 필터(예: `safesearch=off`).

```bash
curl \
  --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
  "https://www.bing.com/search?q=pizza&safesearch=off"
```

**Device Targeting**

- `brd_mobile`: 디바이스 유형을 지정합니다(예: 모바일은 `brd_mobile=1` 또는 `brd_mobile=ios`).

```bash
curl \
  --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
  "https://www.bing.com/search?q=pizza&brd_mobile=1"
```

**Browser Targeting**

- `brd_browser`: 브라우저를 지정합니다(예: `brd_browser=chrome`).

```bash
curl \
  --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
  "https://www.bing.com/search?q=pizza&brd_browser=chrome"
```

**Parsing**

- `brd_json`: 파싱된 JSON을 반환합니다(예: `brd_json=1`).

```bash
curl \
  --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
  "https://www.bing.com/search?q=pizza&brd_json=1"
```

### Yandex
로컬라이제이션, 페이지네이션, 기간, 디바이스/브라우저 타기팅을 위한 매개변수로 Yandex 쿼리를 간단히 구성합니다. [전용 Yandex API](https://brightdata.co.kr/products/serp-api/yandex-search)도 확인하십시오.

**Localization**
- `lr`: 지역을 지정합니다(예: 미국은 `lr=84`).
- `lang`: 페이지 언어(예: `lang=en`).

```bash
curl \
  --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
  "https://www.yandex.com/search/?text=pizza&lr=84&lang=en"
```

**Pagination**
- `p`: 결과 페이지 번호(예: 두 번째 페이지는 `p=2`).

```bash
curl \
  --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
  "https://www.yandex.com/search/?text=pizza&p=2"
```

**Time Range**
- `within`: 기간을 지정합니다(예: `within=1`).

```bash
curl \
  --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
  "https://www.yandex.com/search/?text=pizza&within=1"
```

**Device Targeting**
```bash
curl \
  --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
  "https://www.yandex.com/search/?text=pizza&brd_mobile=1"
```

**Browser Targeting**
```bash
curl \
  --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
  "https://www.yandex.com/search/?text=pizza&brd_browser=chrome"
```

### DuckDuckGo
로컬라이제이션, 안전 검색, 기간, 디바이스/브라우저 타기팅을 사용한 DuckDuckGo 검색 커스터마이즈에 대한 간단한 개요입니다. [전용 DuckDuckGo API](https://brightdata.co.kr/products/serp-api/duckduckgo-search)도 확인하십시오.

**Localization**

- `kl`: 국가 및 언어(예: `kl=us-en`).
- `kad`: 인터페이스 요소의 언어를 정의합니다.

```bash
curl \
  --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
  "https://duckduckgo.com/?q=pizza&kl=us-en"
```

**Safe Search**

- `kp`: 안전 검색을 활성화합니다(예: `kp=1`).

```bash
curl \
  --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
  "https://duckduckgo.com/?q=pizza&kp=1"
```

**Time Range**

- `df`: 기간을 지정합니다(예: `df=d`).

```bash
curl \
  --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
  "https://duckduckgo.com/?q=pizza&df=d"
```

**Device Targeting**

- `brd_mobile`: 모바일 디바이스 에뮬레이션에 사용합니다.

```bash
curl \
  --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
  "https://duckduckgo.com/?q=pizza&brd_mobile=1"
```

**Browser Targeting**

- `brd_browser`: 브라우저를 지정합니다(예: `chrome`).

```bash
curl \
  --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
  "https://duckduckgo.com/?q=pizza&brd_browser=chrome"
```

## Other Settings for SERP API

### Asynchronous Requests
- Sync(기본값): 실시간 응답을 즉시 받습니다.
- Async: 나중에 결과를 조회합니다(대량 リクエ스트에 이상적입니다).

자세히 알아보기: [How Async Works](https://docs.brightdata.com/scraping-automation/serp-api/asynchronous-requests#how-it-works)


### Multi-Query Requests
`multi` 매개변수를 사용하여 하나의 API 호출에서 **병렬 쿼리**를 전송하고, 동일한 IP 주소와 세션을 공유합니다.

```python
multi:[
  {"keyword":"shoes","num":50},
  {"keyword":"shoes","num":200}
]
```
자세히 알아보기: [Multiple Queries Guide](https://docs.brightdata.com/scraping-automation/serp-api/asynchronous-requests#multiple-queries-in-a-single-request)


## Support & Resources
- **Documentation:** [SERP API Docs](https://docs.brightdata.com/scraping-automation/serp-api/)
- **Query Parameters Documentation:** [Detailed Query Parameters Docs](https://docs.brightdata.com/scraping-automation/serp-api/query-parameters)
- **Other Guides:** [Web Unlocker API](https://github.com/bright-kr/web-unlocker-api), [Google Maps](https://github.com/bright-kr/Google-Maps-Scraper), [Google News](https://github.com/bright-kr/Google-News-Scraper)
- **Interesting Read:** [Best SERP APIs](https://brightdata.co.kr/blog/web-data/best-serp-apis), [Build a RAG Chatbot with SERP API](https://brightdata.co.kr/blog/web-data/build-a-rag-chatbot), [Scrape Google Search with Python](https://brightdata.co.kr/blog/web-data/scraping-google-with-python)
- **Technical Support:** [Contact Us](mailto:support@brightdata.com)
# 💱 Currency Converter — Android WebView 貨幣換算 APP

一款以 Android WebView 為核心的多幣別即時換算 APP。
深色介面、手機直式友善，支援 13 種主要亞太與國際貨幣。

---

## 功能特色

- **多幣別同時顯示**：任一欄位輸入數字，其他欄位即時換算更新
- **13 種幣別**，依序如下：

| 國旗 | 幣別代碼 | 中文名稱 |
|------|----------|----------|
| 🇹🇼 | TWD | 台灣新台幣 |
| 🇺🇸 | USD | 美元 |
| 🇪🇺 | EUR | 歐元 |
| 🇭🇰 | HKD | 港幣 |
| 🇨🇳 | CNY | 人民幣 |
| 🇯🇵 | JPY | 日圓 |
| 🇰🇷 | KRW | 韓元 |
| 🇹🇭 | THB | 泰銖 |
| 🇻🇳 | VND | 越南盾 |
| 🇲🇾 | MYR | 馬來西亞令吉 |
| 🇸🇬 | SGD | 新加坡元 |
| 🇮🇩 | IDR | 印尼盾 |
| 🇵🇭 | PHP | 菲律賓披索 |

- **匯率更新時間**顯示（格式：2026年3月20日 15:02）
- 無法連線時自動套用內建估算匯率，離線仍可使用

---

## 專案結構

```
Currency-Converter/
├── app/
│   └── src/main/
│       ├── AndroidManifest.xml
│       ├── assets/
│       │   └── index.html          ← 換算介面主頁
│       ├── java/com/example/currencyconverter/
│       │   └── MainActivity.java
│       └── res/
│           ├── layout/activity_main.xml
│           └── values/
│               ├── strings.xml
│               └── themes.xml
├── .github/workflows/
│   └── build.yml                   ← CI：自動編譯 debug APK
├── build.gradle
├── settings.gradle
└── gradle.properties
```

---

## 設定匯率來源（Cloudflare Worker）

`app/src/main/assets/index.html` 第 14 行：

```javascript
const RATES_URL = 'https://your-worker.workers.dev/rates';
```

將 `https://your-worker.workers.dev/rates` 替換為你的 Cloudflare Worker URL。
Worker 回傳格式（JSON，以任意貨幣為基準均可）：

```json
{
  "rates": {
    "TWD": 1,
    "USD": 0.03125,
    "EUR": 0.02884,
    "JPY": 4.717
  }
}
```

---

## 如何從 GitHub Actions 下載 APK

每次推送到 `main` 分支會自動觸發 CI 編譯。

1. 前往 GitHub repo 頁面
2. 點選上方 **Actions** 分頁
3. 點選最新的 **Build Debug APK** 工作流程執行紀錄
4. 在頁面最下方 **Artifacts** 區塊找到 `currency-converter-debug-<編號>`
5. 點選下載 ZIP，解壓縮後即可得到 `app-debug.apk`
6. 將 APK 安裝至 Android 裝置（需在手機設定中允許「安裝未知來源 App」）

> **注意**：Artifact 預設保留 **30 天**。超過期限請重新觸發 Actions 或手動執行 workflow。

---

## 本地編譯

需要 Android Studio 或已安裝 Android SDK（API 34）。

```bash
# clone 專案
git clone https://github.com/Tigerskin-Hupee/Currency-Converter.git
cd Currency-Converter

# 編譯 debug APK
./gradlew assembleDebug

# APK 位置
# app/build/outputs/apk/debug/app-debug.apk
```

---

## 技術規格

| 項目 | 規格 |
|------|------|
| minSdk | API 21（Android 5.0）|
| targetSdk | API 34（Android 14）|
| 介面 | HTML / CSS / Vanilla JS（WebView） |
| 網路權限 | `INTERNET` |
| 螢幕方向 | 直式鎖定 |

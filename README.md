### HimProxy - 代理系統

[繁體中文](README.md) | [English](README_EN.md)

<blockquote style="color: red;">
  <p><strong>警告！</strong><br>
  HimProxy 僅供測試與體驗之用，我們無法保證其進階安全性和穩定性。</p>
</blockquote>

### HimProxy - 代理系統
<blockquote style="color: lightblue;">
  <p><strong>提示！</strong><br>
  HimProxy 首頁上顯示的 README.md 為最新版的安裝說明。若需查閱特定版本的安裝說明，請下載該版本並參考下載檔案中的 README.md。</p>
</blockquote>

#### 簡介
HimProxy 是一個基於 Python 實現的代理系統，包含三個主要組件：用戶端 (`proxy_client.py`)、代理端 (`proxy_server.py`) 和監測端 (`discovery_service.py`)。該系統旨在提供流量轉發和代理功能，並支援代理狀態的實時基礎監控，適用於需要使用到代理服務的場景，例如匿名瀏覽網站或 IP 代理。

#### 功能特性
- **實時監測與發現**：監測端定期檢查代理端的狀態，自動添加或移除在線/離線的代理。
- **流量監控**：即時顯示客戶端的連線資訊。
- **簡單命令介面**：支援 `stop` 和 `networklist`（代理端）以及 `proxylist` 和 `userlist`（監測端）等指令。

#### 系統架構
- **`proxy_client.py`**：用戶端，負責接收本地請求並轉發至代理端。
- **`proxy_server.py`**：代理端，建議部署於需要代理流量的伺服器上，負責處理流量轉發並與監測端通訊。
- **`discovery_service.py`**：監測端，作為中央節點，管理、分發代理端和用戶端。

#### 安裝需求
1. **環境要求**：
   - Python 3.11 或以上版本
   - 支援的操作系統：Linux / Windows / macOS

2. **安裝依賴**：
   進入對應組件的資料夾，使用以下命令安裝所需依賴：
   ```bash
   pip install -r requirements.txt
   ```

#### 使用方法
1. **啟動監測端**：
   ```bash
   python discovery_service.py
   ```
   - 監測端預設運行於 `localhost:40004`。
   - 可自行修改監測端端口，例如在程式碼中設置 `app.run(host='0.0.0.0', port=8080)`監測端將運行在8080端口上。

2. **配置 HTTPS**：
   - 建議使用 [Cloudflare Tunnels](https://one.dash.cloudflare.com/) 進行配置，以下以 Cloudflare Tunnels 為例：
   - 在 Cloudflare Tunnel 中新增主機名稱，設置類型為 HTTP，URL 填入 `localhost:40004`。如需更改端口，請確保與監測端設置一致。

3. **綁定用戶端與代理端至監測端 URL**：
   - 編輯 `proxy_client.py` 和 `proxy_server.py` 文件，將以下 URL 修改為你的監測端地址：
     - 用戶端：
       - `self.discovery_url = 'https://proxy.example.com/query'`
       - `self.register_url = 'https://proxy.example.com/register_client'`
     - 代理端：
       - `self.discovery_url = 'https://proxy.example.com/register'`
   - 將 `proxy.example.com` 替換為你的實際域名。

4. **設置 API 金鑰**：
   - 在 `proxy_client.py`、`proxy_server.py` 和 `discovery_service.py` 中，自訂 API 金鑰：
     ```python
     self.api_key = "YOURAPIKEY"  # 將 YOURAPIKEY 替換為你自訂的金鑰
     ```
   - 請確保用戶端、代理端及監測端的API一致否則將無法連線。
5. **啟動代理端**：
   ```bash
   python proxy_server.py
   ```
   - 代理端將自動註冊到監測端，並監聽隨機端口。

6. **啟動用戶端**：
   ```bash
   python proxy_client.py
   ```
   - 用戶端將在隨機端口啟動，並將流量轉發至代理端。

7. **測試代理功能**：
   - 在 Windows 11 中，搜尋「Proxy Setting」，選擇「手動 Proxy 設定」，將 Proxy IP 設為用戶端之運行 IP（例如 `localhost`），端口設為用戶端運行端口（例如 `40006`），啟用後儲存即可開始使用。

#### 配置說明
- **端口設置**：
  - 用戶端和代理端預設使用隨機端口（範圍 1024-65535）。如需固定端口，可修改程式碼，例如將：
    ```python
    port = random.randint(1024, 65535)
    ```
    改為：
    ```python
    port = YOUAREPORT #你指定的端口
    ```

#### 注意事項
- **網絡與設備限制**：
  - 代理系統之吞吐量受本地網路環境及硬體性能限制，建議在適合的環境下測試。
- **安全性與穩定性**：
  - 目前採用簡單安全防護，且尚未經過長期穩定性測試，僅適合個人測試用途，不建議使用者正式部署。

#### 貢獻
如有功能建議或發現 Bug，歡迎聯繫我們提交意見。

#### 感謝

#### 許可證
請閱讀`LICENSE`文件。

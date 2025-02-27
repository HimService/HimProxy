### HimProxy - Proxy System

[繁體中文](README.md) | [English](README_EN.md)

<blockquote style="color: red;">
  <p><strong>警告!</strong><br>
  HimProxy所有系統目前正在進行ALPHA測試,僅供測試及體驗,因我們不能保證它的安全性及穩定性,因此我們並不建議用戶長久使用。</p>
</blockquote>

### HimProxy - Proxy System


#### 簡介
這是一個使用 Python 實現的代理系統，包含用戶端 (`proxy_client.py`)、代理端 (`proxy_server.py`) 和監測端 (`discovery_service.py`) 三個主要組件。該系統旨在提供流量轉發功能，並能查看代理狀態。它適用於需要代理服務的場景，例如匿名瀏覽網站，代理ip。


#### 功能特性
- **實時監測發現**：監測端會定期檢測代理端狀態，自動添加或移除在線/離線的代理。
- **流量監控**：實時顯示客戶端連線資訊。
- **簡單命令介面**：支持 `stop`、`networklist`（代理端）和 `proxylist`、`userlist`（監測端）等指令。

#### 系統架構
- **`proxy_client.py`**：用戶端，負責接收本地請求並轉發到代理端。
- **`proxy_server.py`**：代理端，請將他部署在要代理用戶端流量的伺服器上，他將處理流量轉發並與監測端通信。
- **`discovery_service.py`**：監測端，作為中央節點管理代理和客戶端狀態。

#### 安裝需求
1. **環境要求**：
   - Python 3.11+
   - 操作系統：Linux/Windows/macOS

2. **安裝依賴**：
前往要安裝的端點資料夾並使用pip install -r requirements.txt安裝
   ```bash
   pip install -r requirements.txt
   ```

#### 使用方法
1. **其他地區啟動監測端**：
   ```bash
   python discovery_service.py
   ```
   - 監測端將會運行在 `localhost:40004`。
   - 你也可以自行設置監測端端口`app.run(host='0.0.0.0', port=40004)`

2. **配置https**：
   - 我們推薦使用[Cloudflare tunnels](https://one.dash.cloudflare.com/)進行配置,在此也使用Cloudflare tunnels進行示範
   - 在你的tunnel中新增主機名稱,並將類型設置成HTTP連接,URL輸入`localhost:40004`,你也可以自行更改端口但請保持與監測端端口一致。

3. **綁定用戶端與代理端至監測端的url**：
   -前往用戶端 (`proxy_client.py`)與代理端 (`proxy_server.py`)的文件中
   並將
   - `self.discovery_url = 'https://proxy.example.com/query' `
   - `self.register_url = 'https://proxy.example.com/register_client'`
   - 及代理端的
   -  `self.discovery_url = 'https://proxy.example.com/register'`

4. **端口**:客戶端和服務器端口的溝通方式。請在每個(用戶端、代理端和監測端).py檔案中更改
     `self.api_key = "YOURAPIKEY"   #YOURAPIKEY是你自訂的金鑰`

5. **啟動代理端**：
   ```bash
   python proxy_server.py
   ```
   - 代理端將自動註冊到監測端，監聽隨機端口。

6. **在啟動用戶端**：
   ```bash
   python proxy_client.py
   ```
   - 於隨機端口啟用（例如 40006），將流量轉發到代理端。

7. **測試**：
   - 在windows11中搜尋proxy setting並使用`手動Proxy設定`將Proxy IP配置為客戶端運行IP並將連接阜設定為客戶端運行端口（例如 `localhost:40006`）開啟後儲存即可完成設置並開始運作。
#### 配置說明
- **端口**：客戶端和服務器端口隨機選擇，可在代碼中修改（如固定為 40005）。
只需將文件代碼中將`port = random.randint(1024, 65535)`改為`port = random.randint(40005, 40005)`。


#### 注意事項
- **網絡及設備限制**：吞吐量受限於本地網絡和硬件性能，建議在高帶寬環境下測試。
- **安全性及穩定性**：因目前使用簡單的認證，及尚未經過長久的運作測試，僅供個人測試,並不建議用戶正式使用。

#### 貢獻
如果您有新的功能建議或發現 Bug，請聯繫。

#### 感謝

#### 許可證
請閱讀 `LICENSE` 文件。

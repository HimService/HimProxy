### HimProxy - Proxy System

[繁體中文](README.md) | [English](README_EN.md)

<blockquote style="color: red;">
  <p><strong>Warning!</strong><br>
  HimProxy is intended solely for testing and evaluation purposes, and we cannot guarantee its advanced security or stability.</p>
</blockquote>

### HimProxy - Proxy System

<blockquote style="color: lightblue;">
  <p><strong>Tip!</strong><br>
  The README.md displayed on the HimProxy homepage provides the latest installation instructions. To review installation instructions for a specific version, please download that version and refer to the README.md included in the downloaded files.</p>
</blockquote>

#### Introduction
HimProxy is a proxy system implemented in Python, comprising three main components: the client (`proxy_client.py`), the proxy server (`proxy_server.py`), and the discovery service (`discovery_service.py`). This system is designed to provide traffic forwarding and proxy functionalities, with support for real-time monitoring of proxy status. It is suitable for scenarios requiring proxy services, such as anonymous web browsing or IP proxying.

#### Features
- **Real-time Monitoring and Discovery**: The discovery service periodically checks the status of proxy servers, automatically adding or removing proxies based on their online/offline status.
- **Traffic Monitoring**: Displays real-time information about client connections.
- **Simple Command Interface**: Supports commands such as `stop` and `networklist` (for the proxy server) and `proxylist` and `userlist` (for the discovery service).

#### System Architecture
- **`proxy_client.py`**: The client component, responsible for receiving local requests and forwarding them to the proxy server.
- **`proxy_server.py`**: The proxy server component, recommended for deployment on a server requiring proxy traffic. It handles traffic forwarding and communicates with the discovery service.
- **`discovery_service.py`**: The discovery service acts as the central node, managing and distributing proxy servers and clients.

#### Installation Requirements
1. **Environment Requirements**:
   - Python 3.11 or later
   - Supported operating systems: Linux, Windows, macOS

2. **Installing Dependencies**:
   Navigate to the corresponding component folder and install the required dependencies using the following command:
   ```bash
   pip install -r requirements.txt
   ```

#### Usage
1. **Start the Discovery Service**:
   ```bash
   python discovery_service.py
   ```
   - The discovery service runs on `localhost:40004` by default.
   - To modify the discovery service port, update the following line in the code:
     ```python
     app.run(host='0.0.0.0', port=8080)
     ```
     This example changes the discovery service to run on port 8080.

2. **Configure HTTPS**:
   - It is recommended to use [Cloudflare Tunnels](https://one.dash.cloudflare.com/) to enable HTTPS for secure communication. The setup process using Cloudflare Tunnels is as follows:
     - Create a new tunnel in Cloudflare, add a hostname, set the service type to HTTP, and enter the URL `localhost:40004`. If you change the port, ensure it matches the discovery service settings.

3. **Bind Clients and Proxy Servers to the Discovery Service URL**:
   - Edit the `proxy_client.py` and `proxy_server.py` files, replacing the following URLs with your discovery service address:
     - Client:
       - `self.discovery_url = 'https://proxy.example.com/query'`
       - `self.register_url = 'https://proxy.example.com/register_client'`
     - Proxy Server:
       - `self.discovery_url = 'https://proxy.example.com/register'`
   - Replace `proxy.example.com` with your actual domain.

4. **Set the API Key**:
   - In `proxy_client.py`, `proxy_server.py`, and `discovery_service.py`, set a custom API key:
     ```python
     self.api_key = "YOURAPIKEY"  # Replace YOURAPIKEY with your custom key
     ```
   - Ensure that the API key is consistent across the client, proxy server, and discovery service, or the connection will fail.

5. **Start the Proxy Server**:
   ```bash
   python proxy_server.py
   ```
   - The proxy server will automatically register with the discovery service and listen on a random port.

6. **Start the Client**:
   ```bash
   python proxy_client.py
   ```
   - The client will start on a random port and forward traffic to the proxy server.

7. **Test the Proxy Function**:
   - On Windows 11, search for "Proxy Settings," select "Manual Proxy Setup," set the Proxy IP to the client's running IP (e.g., `localhost`), and set the port to the client's running port (e.g., `40006`). Enable the settings and save to start using the proxy.

#### Configuration Guide
- **Port Settings**:
  - By default, the client and proxy server use random ports (range 1024-65535). To set a fixed port, modify the following code:
    ```python
    port = random.randint(1024, 65535)
    ```
    Change it to:
    ```python
    port = YOUR_PORT  # Replace with your specified port
    ```

#### Notes
- **Network and Device Limitations**:
  - The throughput of the proxy system is limited by the local network environment and hardware performance. It is recommended to test in a suitable environment.
- **Security and Stability**:
  - Currently, the system implements basic security measures but has not undergone long-term stability testing. It is intended for personal testing only and is not recommended for production deployment.

#### Contribution
If you have feature suggestions or encounter any bugs, please feel free to contact us and submit your feedback.

#### Acknowledgments

#### License
Please refer to the `LICENSE` file.

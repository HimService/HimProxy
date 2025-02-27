### HimProxy - Proxy System

[繁體中文](README.md) | [English](README_EN.md)

<blockquote style="color: red;">
  <p><strong>Warn!</strong><br>
  All HimProxy systems are currently in ALPHA testing, provided solely for testing and evaluation purposes. As we cannot guarantee their security or stability, we do not recommend long-term use.</p>
</blockquote>

### HimProxy - Proxy System

#### Introduction
This is a proxy system implemented in Python, consisting of three main components: the client (`proxy_client.py`), the proxy server (`proxy_server.py`), and the monitoring service (`discovery_service.py`). The system is designed to provide traffic forwarding functionality while allowing users to view proxy status. It is suitable for scenarios that require proxy services, such as anonymous browsing and proxy IP usage.

#### Features
- **Real-time Monitoring and Discovery**: The monitoring service periodically checks the status of the proxy server and automatically adds or removes proxies as they come online or go offline.
- **Traffic Monitoring**: Displays client connection information in real time.
- **Simple Command Interface**: Supports commands such as `stop` and `networklist` (for the proxy server) as well as `proxylist` and `userlist` (for the monitoring service).

#### System Architecture
- **`proxy_client.py`**: The client, responsible for receiving local requests and forwarding them to the proxy server.
- **`proxy_server.py`**: The proxy server, which should be deployed on the server where client traffic is to be proxied. It handles traffic forwarding and communicates with the monitoring service.
- **`discovery_service.py`**: The monitoring service, which acts as a central node to manage the statuses of proxies and clients.

#### Installation Requirements
1. **Environment Requirements**:
   - Python 3.11+
   - Operating Systems: Linux/Windows/macOS

2. **Installing Dependencies**:  
   Navigate to the folder of the endpoint you wish to install and run:
   ```bash
   pip install -r requirements.txt
   ```

#### Usage Instructions
1. **Starting the Monitoring Service in Other Regions**:
   ```bash
   python discovery_service.py
   ```
   - The monitoring service will run on `localhost:40004`.
   - You can also manually set the monitoring service port using:
     ```python
     app.run(host='0.0.0.0', port=40004)
     ```

2. **Configuring HTTPS**:
   - We recommend using [Cloudflare Tunnels](https://one.dash.cloudflare.com/) for configuration; this example uses Cloudflare Tunnels.
   - In your tunnel, add a hostname and set the type to an HTTP connection. Enter `localhost:40004` as the URL. You can change the port if needed, but please ensure it matches the monitoring service port.

3. **Binding the Client and Proxy Server to the Monitoring Service URL**:
   - In the files for the client (`proxy_client.py`) and the proxy server (`proxy_server.py`), update the URLs as follows:
     - For the client:
       - `self.discovery_url = 'https://proxy.example.com/query'`
       - `self.register_url = 'https://proxy.example.com/register_client'`
     - For the proxy server:
       - `self.discovery_url = 'https://proxy.example.com/register'`

4. **Ports Communication**:  
   In each of the (client, proxy server, and monitoring service) `.py` files, change:
   ```python
   self.api_key = "YOURAPIKEY"   # YOURAPIKEY is your custom key
   ```

5. **Starting the Proxy Server**:
   ```bash
   python proxy_server.py
   ```
   - The proxy server will automatically register with the monitoring service and listen on a random port.

6. **Starting the Client**:
   ```bash
   python proxy_client.py
   ```
   - It will run on a random port (for example, 40006) and forward traffic to the proxy server.

7. **Testing**:
   - On Windows 11, search for "proxy setting" and use the "Manual Proxy Setup" to configure the Proxy IP as the IP on which the client is running, and set the port to the client's running port (for example, `localhost:40006`). Once enabled and saved, the setup is complete and the system will begin operating.

#### Configuration Instructions
- **Ports**: The client and server ports are chosen randomly by default, but they can be modified in the code (for example, fixed to 40005). Simply change the code from:
  ```python
  port = random.randint(1024, 65535)
  ```
  to:
  ```python
  port = random.randint(40005, 40005)
  ```

#### Notes
- **Network and Hardware Limitations**: Throughput is limited by your local network and hardware performance; it is recommended to test in a high-bandwidth environment.
- **Security and Stability**: As the system currently uses simple authentication and has not undergone extensive long-term testing, it is intended solely for personal testing and is not recommended for official use.

#### Contributing
If you have any new feature suggestions or discover any bugs, please get in touch.

#### Acknowledgements

#### License
Plsase see the `LICENSE` file for details.
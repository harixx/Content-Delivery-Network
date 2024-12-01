# Custom CDN Project

This repository provides Python code, bash scripts, and CSV files for implementing a fully functional Content Delivery Network (CDN). The project demonstrates concepts such as DNS, HTTP, caching, network performance optimization, and cloud infrastructure deployment. 

## Features

1. **Core Concepts**:
   - **Content Delivery Network (CDN)**: A system of geographically distributed servers delivering web content efficiently, reducing latency, and improving load times by caching content at edge servers.
   - **Origin Server**: Stores the original content. Edge servers fetch data from here if not cached.
   - **DNS Server**: Maps domain names to IP addresses and determines the optimal edge server for client requests.
   - **HTTP Server**: Edge servers that handle user requests and cache content.

2. **Server Locations**:
   - **Origin Server**: Pre-defined at `cs5700cdnorigin.ccs.neu.edu:8080`.
   - **DNS Server**: Located at `cdn-dns.5700.network`.
   - **HTTP Servers**: Seven servers distributed globally (`cdn-http1.5700.network` to `cdn-http7.5700.network`).

3. **Caching Mechanism**:
   - Disk and memory caching with a 20 MB limit.
   - Utilizes page popularity rankings from `pageviews.csv` (Zipfian distribution).

4. **Tools & Libraries**:
   - Python libraries: `dnslib`, `geoip2`, `BaseHTTPRequestHandler`, `ThreadingMixIn`.
   - Custom scripts and tools for caching, deployment, and performance optimization.

5. **Architecture**:
   - DNS server directs clients to the optimal HTTP server based on geolocation.
   - HTTP servers cache content locally to minimize load on the origin server.

## Files and Scripts

### Python Files:
- **`dnsserver`**: Multi-threaded DNS server that processes DNS queries and maps clients to the closest HTTP server.
- **`httpserver`**: Multi-threaded HTTP server that handles client requests, retrieves content from the origin server, and manages caching.
- **`GeoInfo.py`**: Determines client IP locations and finds the closest HTTP server.
- **`CacheManager.py`**: Manages cache storage and evictions.
- **`utils.py`**: Contains utility functions for HTTP servers.

### CSV Files:
- **`pageviews.csv`**: Lists pages, their popularity, and rankings.
- **`ip.csv`**: Maps IP address blocks to countries.
- **`coordinates.csv`**: Maps countries to average geographic coordinates.

### Deployment Scripts:
- **`deployCDN`**: Prepares and deploys all components to the respective servers.
- **`runCDN`**: Starts the DNS and HTTP servers.
- **`stopCDN`**: Stops all running servers and cleans up resources.

### Cache Setup Scripts:
- **`build_in_memory_cache`**: Prepares a 13 MB in-memory cache for HTTP servers.
- **`build_partial_disk_cache`**: Creates a 7 MB disk cache for HTTP servers.

## Usage

1. **Deploy the CDN**:
   ```bash
   ./deployCDN -p <port> -o <origin> -n <name> -u <username> -i <keyfile>
   ```
   Deploys the DNS and HTTP servers, sets up caching, and configures all components.

2. **Run the CDN**:
   ```bash
   ./runCDN -p <port> -o <origin> -n <name> -u <username> -i <keyfile>
   ```
   Starts the DNS and HTTP servers.

3. **Test the CDN**:
   - DNS server:  
     ```bash
     dig @<dns_ip> -p <dns_port> <domain_name>
     ```
   - HTTP server:  
     ```bash
     curl http://<http_server_ip>:<port>/<page>
     ```

4. **Stop the CDN**:
   ```bash
   ./stopCDN -p <port> -o <origin> -n <name> -u <username> -i <keyfile>
   ```
   Stops all running servers and removes associated files.

## Improvements

1. Integrate network performance tools like **Scamper** for better client-server mappings.
2. Optimize caching by ranking pages based on views per byte.
3. Enhance caching efficiency by distributing download loads among HTTP servers. 

This project was tested on **Ubuntu 20.04.1**. 

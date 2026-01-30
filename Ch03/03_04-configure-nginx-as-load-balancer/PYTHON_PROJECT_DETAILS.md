# Python Project Details

A simple Python script that starts multiple HTTP servers using Python's built-in `http.server` module. This script is designed to work with NGINX reverse proxy configurations for testing and development purposes.

## Features

- Native Python implementation using `http.server` framework
- Multi-process server support using `os.fork()`
- Simple HTML response pages showing port number and current time
- Configurable port list for multiple server instances
- Graceful shutdown handling with `KeyboardInterrupt` support
- No external dependencies required for running the server

## Prerequisites

- Python >= 3.6 (uses standard library only)
- pip (Python package manager) - for development dependencies only

## Installation

> [!NOTE]
> This script uses only Python standard library modules. No installation is required to run the server.

For development tools (formatting and linting):

```bash
make development
```

Or manually:

```bash
pip install --requirement requirements.txt
```

## Configuration

The script can be configured by modifying the `PORTS` list in `start_app_servers.py`:

```python
PORTS = [7001]  # Add more ports as needed, e.g., [7001, 7002, 7003]
```

The `hostName` variable can also be modified:

```python
hostName = "localhost"  # Change to "0.0.0.0" to listen on all interfaces
```

## Running the Application

### Basic Usage

```bash
python3 start_app_servers.py
```

Or make it executable and run directly:

```bash
chmod +x start_app_servers.py
./start_app_servers.py
```

The script will:

1. Start HTTP servers on each port specified in the `PORTS` list
2. Fork a separate process for each server
3. Display startup messages with timestamps
4. Wait for `KeyboardInterrupt` (CTRL+C) to stop all servers

### Accessing the Servers

Once running, you can access the servers at:

- `http://localhost:7001` (or any port in the PORTS list)

Each server responds with an HTML page showing:

- The server's port number
- The current time

## API Endpoints

### GET Request

|Route|Description|Status Code|
|-----|-----------|-----------|
|**GET** `/`|Returns an HTML page displaying the server port and current time.|`200 OK`|

**Response:**

```html
<!DOCTYPE html>
<html>
    <head>
        <style>...</style>
        <title>localhost:7001</title>
    </head>
    <body>
        <h1>7001</h1>
        <h1>14:30:45</h1>
    </body>
</html>
```

The response includes:

- The server's port number as a large heading
- The current time (HH:MM:SS format) as a large heading
- Styled with centered text and large font sizes

## Project Structure

```bash
.
├── start_app_servers.py      # Main application script
├── requirements.txt           # Development dependencies (black, flake8)
├── Makefile                  # Build and quality check commands
├── binaryville.conf          # NGINX configuration file
└── README.md                 # Project README
```

## Use Case

This script is designed to work with NGINX reverse proxy configurations. It provides simple backend servers that can be proxied through NGINX for testing and demonstration purposes.

### Example NGINX Configuration

```nginx
upstream app_server_7001 {
    server 127.0.0.1:7001;
}

server {
    location /proxy {
        proxy_pass http://app_server_7001/;
    }
}
```

## Troubleshooting

### Port Already in Use

If you get an error that a port is already in use:

1. Change the port in the `PORTS` list in `start_app_servers.py`
2. Or stop the process using the port:

   ```bash
   lsof -ti:7001 | xargs kill
   ```

### Permission Denied

If you get a permission denied error:

1. Ensure you have permission to bind to the port (ports < 1024 require root)
2. Use ports >= 1024 for non-root execution
3. Check that the port is not already in use

### Fork Errors

If you encounter fork-related errors:

1. Ensure your system supports `os.fork()` (Unix/Linux/macOS)
2. Check system resource limits (ulimit)
3. Verify you're not exceeding process limits

### Import Errors

Since this script uses only standard library modules, import errors are unlikely. If they occur:

1. Verify Python version: `python3 --version` (should be >= 3.6)
2. Check that Python standard library is intact
3. Ensure you're using `python3` not `python2`

### Stopping the Servers

Press `CTRL+C` to stop all servers gracefully. The script will:

- Catch the `KeyboardInterrupt`
- Close all server connections
- Display shutdown messages with timestamps
- Exit cleanly

## Notes

- This script does not include a test suite
- The `requirements.txt` file contains only development tools (black, flake8)
- No production dependencies are required - the script runs using Python standard library only
- The script uses `os.fork()` which is Unix/Linux/macOS specific (not available on Windows)
- Each server runs in its own process, allowing multiple servers to run simultaneously

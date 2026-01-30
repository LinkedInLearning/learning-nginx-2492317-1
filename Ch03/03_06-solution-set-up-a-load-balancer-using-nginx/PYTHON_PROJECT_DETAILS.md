# Python Project Details

A simple Python script that starts multiple HTTP servers using Python's built-in `http.server` module. This script generates unique UUIDs for each request and is designed to work with NGINX load balancer configurations for testing and development purposes.

## Features

- Native Python implementation using `http.server` framework
- Multi-process server support using `os.fork()`
- UUID generation for each request to demonstrate load balancing
- Simple HTML response pages showing generator number and unique UUID
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

The script can be configured by modifying the `PORTS` list in `uuid-generator.py`:

```python
PORTS = [9001, 9002, 9003]  # Add or remove ports as needed
```

The `hostName` variable can also be modified:

```python
hostName = "localhost"  # Change to "0.0.0.0" to listen on all interfaces
```

## Running the Application

### Basic Usage

```bash
python3 uuid-generator.py
```

Or make it executable and run directly:

```bash
chmod +x uuid-generator.py
./uuid-generator.py
```

The script will:

1. Start HTTP servers on each port specified in the `PORTS` list
2. Fork a separate process for each server
3. Display startup messages with timestamps
4. Wait for `KeyboardInterrupt` (CTRL+C) to stop all servers

### Accessing the Servers

Once running, you can access the servers at:

- `http://localhost:9001`
- `http://localhost:9002`
- `http://localhost:9003`

Each server responds with an HTML page showing:

- The generator number (port number)
- A unique UUID generated for each request

## API Endpoints

### GET Request

|Route|Description|Status Code|
|-----|-----------|-----------|
|**GET** `/`|Returns an HTML page displaying the generator number and a unique UUID.|`200 OK`|

**Response:**

```html
<!DOCTYPE html>
<html>
    <head>
        <style>...</style>
        <title>localhost:9001</title>
    </head>
    <body>
        <h1>Generator #9001</h1>
        <h1>UUID=550e8400-e29b-41d4-a716-446655440000</h1>
    </body>
</html>
```

The response includes:

- The generator number (server's port number) as a large heading
- A unique UUID (version 4) generated for each request as a large heading
- Styled with centered text and large font sizes

**Note:** Each request generates a new UUID, making it easy to verify that load balancing is working correctly when requests are distributed across multiple servers.

## Project Structure

```bash
.
├── uuid-generator.py         # Main application script
├── requirements.txt           # Development dependencies (black, flake8, isort)
├── Makefile                  # Build and quality check commands
├── load_balancer.conf        # NGINX load balancer configuration file
└── README.md                 # Project README
```

## Use Case

This script is designed to work with NGINX load balancer configurations. It provides multiple backend servers that generate unique UUIDs, making it easy to verify that load balancing is distributing requests across all servers correctly.

### Example NGINX Configuration

```nginx
upstream uuid {
    server 127.0.0.1:9001;
    server 127.0.0.1:9002;
    server 127.0.0.1:9003;
}

server {
    listen 80;

    location /uuid {
        proxy_pass http://uuid/;
    }
}
```

When accessing the NGINX server at `/uuid`, requests will be distributed across the three backend servers (ports 9001, 9002, and 9003), and each response will show a different generator number and UUID, confirming that load balancing is working.

## Troubleshooting

### Port Already in Use

If you get an error that a port is already in use:

1. Change the port in the `PORTS` list in `uuid-generator.py`
2. Or stop the process using the port:

   ```bash
   lsof -ti:9001 | xargs kill
   lsof -ti:9002 | xargs kill
   lsof -ti:9003 | xargs kill
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
2. Check that Python standard library is intact (including `uuid` module)
3. Ensure you're using `python3` not `python2`

### Stopping the Servers

Press `CTRL+C` to stop all servers gracefully. The script will:

- Catch the `KeyboardInterrupt`
- Close all server connections
- Display shutdown messages with timestamps
- Exit cleanly

## Notes

- This script does not include a test suite
- The `requirements.txt` file contains only development tools (black, flake8, isort)
- No production dependencies are required - the script runs using Python standard library only
- The script uses `os.fork()` which is Unix/Linux/macOS specific (not available on Windows)
- Each server runs in its own process, allowing multiple servers to run simultaneously
- UUIDs are generated using Python's `uuid.uuid4()` function, which creates random UUIDs (version 4)
- Each request generates a new UUID, making it ideal for load balancing demonstrations

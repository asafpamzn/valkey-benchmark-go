# Valkey Benchmark GO

Valkey Benchmark GO is a benchmark tool for Valkey. This tool allows developers to conduct performance testing in GO while maintaining consistent testing methodologies across different implementations. It supports various testing scenarios, including throughput testing, latency measurements, and custom command benchmarking.

Key Advantages:

1. **Enhanced GO Developer Experience** Built with native Valkey clients in GO, providing authentic end-to-end performance measurements in your production environment.

2. **Advanced Cluster Testing**: Leverages the [valkey-GLIDE](https://github.com/valkey-io/valkey-glide) client to support:
   - Dynamic cluster topology updates
   - Read replica operations
  
3. **Extensible Command Framework**: 
   - Supports custom commands and complex operations
   - Test Lua scripts and multi-key transactions
   - Benchmark your specific use cases and data patterns

## Installation

1. Clone the repository:
    ```bash
    git clone <repository-url>
    cd valkey-benchmark-go
    ```

2. Install dependencies:
    ```bash
    go mod init valkey-benchmark
    go get github.com/valkey-io/valkey-glide/go
    go mod tidy
    
    ```

## Basic Usage

Run a basic benchmark:
```bash
go build
./valkey-benchmark -H localhost -p 6379
```

Common usage patterns:
```bash
# Run SET benchmark with 50 parallel clients
./valkey-benchmark -c 50 -t set

# Run GET benchmark with rate limiting
./valkey-benchmark -t get --qps 1000

# Run benchmark for specific duration
./valkey-benchmark --test-duration 60

# Run benchmark with sequential keys
./valkey-benchmark --sequential 1000000
```

## Configuration Options

### Basic Options
- `-H, --host <hostname>`: Server hostname (default: "127.0.0.1")
- `-p, --port <port>`: Server port (default: 6379)
- `-c, --clients <num>`: Number of parallel connections (default: 50)
- `-n, --requests <num>`: Total number of requests (default: 100000)
- `-d, --datasize <bytes>`: Data size for SET operations (default: 3)
- `-t, --type <command>`: Command to benchmark (e.g., SET, GET)

### Advanced Options
- `--threads <num>`: Number of worker threads (default: 1)
- `--test-duration <seconds>`: Run test for specified duration
- `--sequential <keyspace>`: Use sequential keys
- `-r, --random <keyspace>`: Use random keys from keyspace

### Rate Limiting Options
- `--qps <num>`: Limit queries per second
- `--start-qps <num>`: Starting QPS for dynamic rate
- `--end-qps <num>`: Target QPS for dynamic rate
- `--qps-change-interval <seconds>`: Interval for QPS changes
- `--qps-change <num>`: QPS change amount per interval

### Security Options
- `--tls`: Enable TLS connection

### Cluster Options
- `--cluster`: Use cluster client
- `--read-from-replica`: Read from replica nodes

## Output Format

The benchmark tool provides real-time statistics during execution:

```
Progress: 1234 requests, Current RPS: 1234.56, Overall RPS: 1230.45, Errors: 0
Latencies (ms) - Avg: 0.12, p50: 0.11, p99: 0.18
```

Final results include:
- Total execution time
- Total requests completed
- Average requests per second
- Error count
- Latency statistics (min, avg, max, p50, p95, p99)

## Dependencies

This tool requires:
- Go 1.21 or higher
- [github.com/valkey-io/valkey-glide](https://github.com/valkey-io/valkey-glide) - Valkey GLIDE client library

## Examples

### Basic SET Benchmark
```bash
./valkey-benchmark -H localhost -p 6379 -t set -n 100000
```

### GET Benchmark with Rate Limiting
```bash
./valkey-benchmark -H localhost -p 6379 -t get --qps 5000
```

### Cluster Benchmark
```bash
./valkey-benchmark -H localhost -p 6379 --cluster --read-from-replica
```

### High Concurrency Test
```bash
./valkey-benchmark -H localhost -p 6379 -c 200 -n 1000000
```
## Custom Benchmark Commands

The benchmark tool supports custom command execution for more complex testing scenarios. The custom command implementation performs concurrent HMGET operations in batches, which is useful for testing real-world workload patterns.

### Custom Command Details

When using the `-t custom` option, the benchmark will:
- Execute the code in CustomCommandStandalone or CustomCommandCluster. Update the code and build the project to test custom scenarios.

### Usage Examples

```bash
# Run custom benchmark in standalone mode
./valkey-benchmark -t custom -H localhost -p 6379

# Run custom benchmark in cluster mode
./valkey-benchmark -t custom -H localhost -p 6379 --cluster

# Run custom benchmark with specific QPS
./valkey-benchmark -t custom -H localhost -p 6379 --qps 1000

# Run custom benchmark with multiple threads
./valkey-benchmark -t custom -H localhost -p 6379 --threads 4
```
## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

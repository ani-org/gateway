# Agent Gateway Deployment Guide

SmartAGW is the Agent Gateway software. The executable `main.bin` is located in the software directory.

## 📋 Environment Requirements

Linux x86 environment. No additional dependencies required.

## SmartAGW Configuration

The configuration file for SmartAGW is `.env`, which contains multiple parameters. The key parameters used are listed below:

1. **Gateway ID (must be modified)**
Each AGW must have a unique ID. It is recommended to name it using the abbreviation of your university.

```
SERVICE_GATEWAY_ID=gw-A
```

2. **Advertised IP address (must be modified)**
This is the externally exposed address. 

```
DATA_PLANE_ADVERTISE_HOST=10.14.0.1   # Data plane advertise address
```

3. **Address of the lookup server**
You may use the public Search service below for testing, or deploy your own Search service (recommended for better stability).

```
HTTP_URL="http://127.0.0.1:9011"
```

## How to Run SmartAGW

```
chmod +x main.bin
./main.bin
```

### Run in background (using nohup, continues running after SSH disconnect)

```
# Redirect stdout and stderr into main.log
nohup ./main.bin > main.log 2>&1 &
```

- `> main.log`: Write standard output to log file
- `2>&1`: Redirect error output to the same log file
- Trailing `&`: Run the process in background
- The program keeps running after closing SSH or terminal

View real-time logs:

```
tail -f main.log
```

Run without log output:

```
nohup ./main.bin > /dev/null 2>&1 &
```

Check and stop the process:

```
# Find the PID
ps aux | grep main.bin

# Graceful stop
kill <YOUR_PID>
# Force kill (use when stuck)
kill -9 <PID>
```

## WEB UI for AGW Status and Gateway Management

```
HTTP_URL=http://<LOCAL_IP>:5000
```

Notes: The port depends on the configuration in the .env file. 

### Neighbor Management

Neighbors can be configured manually. A connection can only be established when both AGWs add each other as neighbors (similar to BGP peer configuration).

- Default control plane port for neighbor gateways: `17000`
- Default data plane port for neighbor gateways: `19000`

Notes: The port depends on the configuration in the .env file. 

### Agent Management

The gateway supports manual revocation of Agents and manual revocation of Agent announcements.

# Project Overview

This project provides a “monitored app” and a “monitoring agent” setup using Docker. It includes two separate Docker Compose configurations:
1. The “target-app” – A simple application that runs on port 3000 (mapped to 5000 internally).
2. The “monitor-agent” – This consists of two services: 
   - Ollama (an LLM container)  
   - Monitor agent (to monitor the target app using the LLM)

By running both Docker Compose files, you get a minimal environment where the target app is running and the monitor can track and provide insights on that app.

---

## Prerequisites

- Docker and Docker Compose should be installed and running on your system.
- Enough system resources to run both containers (the target app and the monitor agent).
- start by cloning 
`bash
https://github.com/sambhavnoobcoder/Docker-Monitor.git
`
---
## Contents

1. [Folder Structure](#folder-structure)  
2. [Usage Guide](#usage-guide)  
3. [Configuration](#configuration)  
4. [How It Works](#how-it-works)  
5. [Troubleshooting](#troubleshooting)  
6. [License](#license)

---

## Folder Structure
.
├── monitor-agent/
│ ├── docker-compose.yml
│ └── ...
└── target-app/
├── docker-compose.yml
└── ...

- The monitor-agent folder contains all files needed to run the monitoring setup (Ollama container and the monitor agent).
- The target-app folder contains files needed to host the sample application to be monitored.

---

## Usage Guide

Follow these steps to start using the project:

1. Clone the repository or download the files.  
2. Navigate to the folder where the target app is defined:
   ```bash
   cd target-app
   ```
3. Bring up the target app using docker-compose:
   ```bash
   docker compose up
   ```
   - This starts the container named “dist-monitored-app” on port 3000 (internally mapped to port 5000).  
4. Open a new terminal window/tab and go to the monitor-agent folder:
   ```bash
   cd ../monitor-agent
   ```
5. Bring up the monitor agent services:
   ```bash
   docker compose up
   ```
   - This starts two services:
     - ollama (LLM container) on port 11435 (mapped to 11434 internally)  
     - dist-monitor-agent, which communicates with both your target-app and ollama.  

After these steps, you should have:
- The target app listening on http://localhost:3000
- The monitor agent running in the background tracking the target app.

---

## Configuration

You can customize the setup by modifying environment variables in the respective docker-compose files:

• In monitor-agent/docker-compose.yml:
- OLLAMA_HOST: Changes the host on which the Ollama LLM container listens. Default is http://dist-ollama:11434.  
- TARGET_HOST: The host of the target application. Default is host.docker.internal.  
- TARGET_PORT: Port to reach the target application. Default is 3000.  

• In target-app/docker-compose.yml:
- The external port that maps to the internal container port for the target app (default “3000:5000”).

Feel free to adjust any of these as needed for your environment.

---

## How It Works

1. The “target-app” is a sample application running on port 5000 internally, exposed to your machine on port 3000.  
2. The “monitor-agent” uses two containers:  
   - Ollama (for LLM processing)  
   - Monitor agent (fetches data from the target app and sends it to Ollama for potential analysis or logging).  
3. Requests to the target-app can be observed or analyzed in real time by the monitor agent’s logic (could be expanded for logging, AI-based responses, or other behavior).
---

## Test cmds 
# Test health endpoint
curl http://localhost:5000/api/health

### Trigger an issue
curl http://localhost:5000/api/trigger-issue

### Check health again
curl http://localhost:5000/api/health
---

## Troubleshooting

1. Make sure Docker is installed and running.  
2. Check that the ports are free (3000 for the target app, 11435 for the Ollama container).  
3. Run logs to see any container errors:  
   ```bash
   docker compose logs -f
   ```
   (Run this in either “monitor-agent” or “target-app” folder to view their container logs.)
4. If you encounter issues, check the docker-compose.yml files for any misconfigurations.
5. if you find the port is not free, you can change the port in the docker-compose.yml file.
---

**Happy Coding!** If you have any questions or issues, please feel free to open an issue in the repository or contact the maintainers.

# EX01 Developing a Simple Webserver
## Date:26/09/2024

## AIM:
To develop a simple webserver to serve html pages and display the configuration details of laptop.

## DESIGN STEPS:
### Step 1: 
HTML content creation.

### Step 2:
Design of webserver workflow.

### Step 3:
Implementation using Python code.

### Step 4:
Serving the HTML pages.

### Step 5:
Testing the webserver.

## PROGRAM:
```
import http.server
import socketserver
import platform
import psutil
import json

PORT = 8000

class MyHandler(http.server.SimpleHTTPRequestHandler):
    def do_GET(self):
        if self.path == '/config':
            self.send_response(200)
            self.send_header('Content-type', 'application/json')
            self.end_headers()

          
            config_details = {
                "OS": platform.system(),
                "OS Version": platform.version(),
                "Machine": platform.machine(),
                "Processor": platform.processor(),
                "CPU Cores": psutil.cpu_count(logical=True),
                "RAM (GB)": round(psutil.virtual_memory().total / (1024**3), 2),
                "Disk Space (GB)": round(psutil.disk_usage('/').total / (1024**3), 2),
                "Disk Usage (%)": psutil.disk_usage('/').percent,
                "Battery": psutil.sensors_battery().percent if psutil.sensors_battery() else 'N/A',
            }

           
            self.wfile.write(json.dumps(config_details).encode('utf-8'))

        else:
            # Serve a simple HTML page
            self.send_response(200)
            self.send_header('Content-type', 'text/html')
            self.end_headers()
            self.wfile.write(b"<html><body><h1>Welcome to Laptop Config Server</h1><p><a href='/config'>Check Laptop Configuration</a></p></body></html>")


with socketserver.TCPServer(("", PORT), MyHandler) as httpd:
    print(f"Serving on port {PORT}")
    httpd.serve_forever()
```
## OUTPUT:
![Screenshot 2024-09-26 181842](https://github.com/user-attachments/assets/6f838876-085d-4386-9020-79217542c1d6)

![Screenshot 2024-09-26 182249](https://github.com/user-attachments/assets/bc2d0cad-b337-4d4c-b15b-8ff9e628cce9)


## RESULT:
The program for implementing simple webserver is executed successfully.

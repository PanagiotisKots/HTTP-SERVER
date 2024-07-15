# HTTP-SERVER
## Simple Linux Http Server in C
<br>

### Description

This project is a simple HTTP server written in C for Linux. It supports multithreading to handle multiple client connections simultaneously and provides a command-line interface for server management. 
The server can be started, paused, resumed, and stopped through an easy and user-friendly menu. Additionally, it includes signal handling to gracefully stop the server using SIGINT or SIGTERM.


<br>

### Features

    Multithreading: Each client connection is handled in a separate thread.
    Signal Handling: Gracefully stops the server on SIGINT or SIGTERM.
    Command Line Interface: Provides a menu to control the server.
    Mutex Locking: Ensures thread-safe operations on server state.

### Requirements

    GCC (GNU Compiler Collection) 
    

### Compilation

To compile the server, run the following command in your terminal:

gcc http_server.c -o http_server 

### Usage

After compiling the server, you can run the executable:

sudo ./http_server

You will be presented with a menu to control the server:


# --- Simple HTTP Server Menu ---
## 1. Start Server
## 2. Pause Server
## 3. Resume Server
## 4. Stop Server
## 5. Exit
## Select an option: 

### Options

    Start Server: Starts the HTTP server and listens for incoming connections on port 4040.
    Pause Server: Pauses the server, temporarily stopping it from accepting new connections.
    Resume Server: Resumes the server if it is paused.
    Stop Server: Stops the server and closes the listening socket.
    Exit: Stops the server (if running) and exits the program.











Steps to Connect from Windows to Linux Server:

    Find the Server's IP Address:

        On your Linux server machine, determine its IP address using the ifconfig command:


    ifconfig

    Look for the IP address associated with your network interface (e.g., eth0, wlan0). 
    
    (!)Note down this IP address.

Run the Server:

    Start your HTTP server program on the Linux machine as usual, ensuring it listens on a specified port (e.g., port 4040).


    ./http_server

Connect from Windows Machine:

    On your Windows machine, open a web browser (such as Chrome, Firefox, or Edge).

    In the address bar, type the IP address of your Linux server followed by the port number where the server is running. 
    

    http://<linux-server-ip>:4040

Replace <linux-server-ip> with the actual IP address of your Linux machine.

Example: If your Linux server's IP address is 192.168.1.100 and your server is running on port 4040, you would type:


        http://192.168.1.100:4040

    Access the Web Page:
        Press Enter or Go in the web browser's address bar. If everything is set up correctly, 
        you should see the webpage served by your Linux HTTP server.



    Firewall: Ensure that the firewall on your Linux machine allows incoming connections on port 4040. You may need to configure iptables or ufw depending on your Linux distribution.
    Network Configuration: Both machines should be on the same local network for direct communication without additional configuration.

This approach allows you to access the webpage served by your Linux-based HTTP server from any machine (including Windows) within your local network, as long as you know the server's IP address and the port it is listening on.

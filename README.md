
# Simple Linux Http Server in C
<br>

### Description

This project is a lightweight HTTP server written in 🎓C for 📟Linux, designed with multithreading to 🧬efficiently manage multiple client connections concurrently. It features a command-line interface for straightforward 🌟server control, allowing you to start, pause, resume, and stop the server via a user-friendly menu. The server also includes 🕹️robust signal handling, enabling graceful shutdown with SIGINT or SIGTERM.


<br>

## Features

 + __Multithreading:__ Each client connection is handled in a separate thread.<br>
 +  __Signal Handling:__  Gracefully stops the server on SIGINT or SIGTERM.<br>
 +  __Command Line Interface:__  Provides a menu to control the server.<br>
 +  __Mutex Locking:__  Ensures thread-safe operations on server state.


    

## Compilation & Usage
⚠️**__It is necessary to install GCC (GNU Compiler Collection)__**

+ To compile the server, run the following command in your terminal:

` gcc http_server.c -o http_server `



+ After compiling the server, you can run the executable:

`sudo ./http_server`

▶️  __You will be presented with a menu to control the server:__


# --- Simple HTTP Server Menu ---
## 1. Start Server
## 2. Pause Server
## 3. Resume Server
## 4. Stop Server
## 5. Exit
 ### 🧾Select an option: 



+ __Start Server:__ Starts the HTTP server and listens for incoming connections on port 4040.
+ __Pause Server:__ Pauses the server, temporarily stopping it from accepting new connections.
+ __Resume Server:__ Resumes the server if it is paused.
+ __Stop Server:__ Stops the server and closes the listening socket.
+ __Exit:__ Stops the server (if running) and exits the program.











## Steps to Connect from Windows to Linux Server:

### Firstly, you have to find find the Server's IP Address:

   + On your Linux server machine, determine its IP address with the following command:


 `ifconfig`

+ Look for the IP address associated with your network interface (e.g., eth0, wlan0). 
    
   ✍️Note down this IP address.

### When, Run the Server:

   🔧Start your HTTP server program on the Linux machine as usual, ensuring it listens on a specified port (e.g., port 4040).


 ` ./http_server`

+ Connect from Windows Machine:

    On your Windows machine, open a web browser (such as Chrome, Firefox, or Edge).

    In the address bar, type the IP address of your Linux server followed by the port number where the server is running. 
    

 `http://<linux-server-ip>:4040`

###  Replace it <linux-server-ip> with the actual IP address of your Linux machine.

*⚠️ __For Example:__ If your Linux server's IP address is 192.168.1.100 and your server is running on port 4040, you would type: http://192.168.1.100:4040 *



 + 🌐 __Access the Web Page:__
 Press Enter or Go in the web browser's address bar. If everything is set up correctly, 
  you should see the webpage served by your Linux HTTP server.



 🔥 __Firewall:__ Ensure that the firewall on your Linux machine allows incoming connections on port 4040. You may need to configure iptables or ufw depending on your Linux distribution.
    Network Configuration: Both machines should be on the same local network for direct communication without additional configuration.

# Conclusion
This method enables you to access the webpage hosted on your Linux-based HTTP server from any device within your local network, including Windows machines, as long as you have the server's IP address and the correct port number it's using.

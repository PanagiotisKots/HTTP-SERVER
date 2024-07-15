#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <pthread.h>
#include <signal.h>
#include <arpa/inet.h>

#define PORT 4040
#define BUFFER_SIZE 1024
#define BACKLOG 10

typedef enum {
    SERVER_STOPPED,
    SERVER_RUNNING,
    SERVER_PAUSED
} ServerState;

volatile ServerState server_state = SERVER_STOPPED;
int server_socket;
pthread_mutex_t server_mutex = PTHREAD_MUTEX_INITIALIZER;

void handle_client(int client_socket) {
    char buffer[BUFFER_SIZE];
    int bytes_read = recv(client_socket, buffer, BUFFER_SIZE - 1, 0);

    if (bytes_read < 0) {
        perror("Error reading from socket");
        close(client_socket);
        return;
    }

    buffer[bytes_read] = '\0';
    printf("Received request:\n%s\n", buffer);

    const char *response =
        "HTTP/1.1 200 OK\r\n"
        "Content-Type: text/html\r\n"
        "Connection: close\r\n"
        "\r\n"
        "<!DOCTYPE html>"
        "<html>"
        "<head><title>Simple HTTP Server</title></head>"
        "<body><h1>Hello, World!</h1></body>"
        "</html>";

    send(client_socket, response, strlen(response), 0);
    close(client_socket);
}

void* server_thread(void* arg) {
    struct sockaddr_in client_addr;
    socklen_t client_addr_len = sizeof(client_addr);

    printf("Server is running on port %d\n", PORT);

    while (1) {
        pthread_mutex_lock(&server_mutex);
        if (server_state == SERVER_STOPPED) {
            pthread_mutex_unlock(&server_mutex);
            break;
        } else if (server_state == SERVER_PAUSED) {
            pthread_mutex_unlock(&server_mutex);
            sleep(1); // Wait for a bit before checking again
            continue;
        }
        pthread_mutex_unlock(&server_mutex);

        int client_socket = accept(server_socket, (struct sockaddr*)&client_addr, &client_addr_len);
        if (client_socket < 0) {
            perror("Accept failed");
            continue;
        }

        pthread_t client_thread;
        pthread_create(&client_thread, NULL, (void* (*)(void*))handle_client, (void*)(intptr_t)client_socket);
        pthread_detach(client_thread);
    }

    return NULL;
}

void start_server() {
    if (server_state != SERVER_STOPPED) {
        printf("Server is already running or paused.\n");
        return;
    }

    struct sockaddr_in server_addr;

    server_socket = socket(AF_INET, SOCK_STREAM, 0);
    if (server_socket < 0) {
        perror("Socket creation failed");
        exit(EXIT_FAILURE);
    }

    server_addr.sin_family = AF_INET;
    server_addr.sin_addr.s_addr = INADDR_ANY;
    server_addr.sin_port = htons(PORT);

    if (bind(server_socket, (struct sockaddr*)&server_addr, sizeof(server_addr)) < 0) {
        perror("Bind failed");
        close(server_socket);
        exit(EXIT_FAILURE);
    }

    if (listen(server_socket, BACKLOG) < 0) {
        perror("Listen failed");
        close(server_socket);
        exit(EXIT_FAILURE);
    }

    server_state = SERVER_RUNNING;
    pthread_t server_thread_id;
    pthread_create(&server_thread_id, NULL, server_thread, NULL);
    pthread_detach(server_thread_id);
}

void pause_server() {
    pthread_mutex_lock(&server_mutex);
    if (server_state == SERVER_RUNNING) {
        server_state = SERVER_PAUSED;
        printf("Server paused.\n");
    } else {
        printf("Server is not running.\n");
    }
    pthread_mutex_unlock(&server_mutex);
}

void resume_server() {
    pthread_mutex_lock(&server_mutex);
    if (server_state == SERVER_PAUSED) {
        server_state = SERVER_RUNNING;
        printf("Server resumed.\n");
    } else {
        printf("Server is not paused.\n");
    }
    pthread_mutex_unlock(&server_mutex);
}

void stop_server() {
    pthread_mutex_lock(&server_mutex);
    if (server_state != SERVER_STOPPED) {
        server_state = SERVER_STOPPED;
        close(server_socket);
        printf("Server stopped.\n");
    } else {
        printf("Server is not running.\n");
    }
    pthread_mutex_unlock(&server_mutex);
}

void display_menu() {
    printf("\n--- Simple HTTP Server Menu ---\n");
    printf("1. Start Server\n");
    printf("2. Pause Server\n");
    printf("3. Resume Server\n");
    printf("4. Stop Server\n");
    printf("5. Exit\n");
    printf("Select an option: ");
}

void handle_signal(int signal) {
    if (signal == SIGINT || signal == SIGTERM) {
        printf("\nReceived signal to stop the server.\n");
        stop_server();
        exit(EXIT_SUCCESS);
    }
}

int main() {
    signal(SIGINT, handle_signal);
    signal(SIGTERM, handle_signal);

    int option;
    do {
        display_menu();
        if (scanf("%d", &option) != 1) {
            printf("Invalid input. Please enter a number.\n");
            while (getchar() != '\n'); // Clear invalid input
            continue;
        }
        getchar(); // To consume the newline character left by scanf

        switch (option) {
            case 1:
                start_server();
                break;
            case 2:
                pause_server();
                break;
            case 3:
                resume_server();
                break;
            case 4:
                stop_server();
                break;
            case 5:
                stop_server();
                exit(EXIT_SUCCESS);
            default:
                printf("Invalid option. Please try again.\n");
        }
    } while (1);

    return 0;
}

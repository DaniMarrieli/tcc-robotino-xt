O código rodando no robotino está abaixo:

#include <iostream>
#include <cstring>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <unistd.h>

#define PORT 5000
#define BUFFER_SIZE 1024

int main() {
    int server_fd, new_socket;
    struct sockaddr_in address;
    int addrlen = sizeof(address);
    char buffer[BUFFER_SIZE] = {0};
    const char* hello = "Hello from Robotino server";

    // Criar socket
    if ((server_fd = socket(AF_INET, SOCK_STREAM, 0)) == 0) {
        perror("socket failed");
        return -1;
    }

    // Configurar endereço
    address.sin_family = AF_INET;
    address.sin_addr.s_addr = INADDR_ANY; // aceita conexões de qualquer IP
    address.sin_port = htons(PORT);

    // Bind socket à porta
    if (bind(server_fd, (struct sockaddr *)&address, sizeof(address)) < 0) {
        perror("bind failed");
        close(server_fd);
        return -1;
    }

    // Escutar conexões
    if (listen(server_fd, 3) < 0) {
        perror("listen");
        close(server_fd);
        return -1;
    }

    std::cout << "Servidor TCP rodando na porta " << PORT << std::endl;

    while (true) {
        // Aceitar conexão
        if ((new_socket = accept(server_fd, (struct sockaddr )&address, (socklen_t)&addrlen)) < 0) {
            perror("accept");
            close(server_fd);
            return -1;
        }

        std::cout << "Cliente conectado" << std::endl;

        // Receber dados
        while (true) {
            int valread = read(new_socket, buffer, BUFFER_SIZE);
            if (valread <= 0) {
                std::cout << "Cliente desconectado" << std::endl;
                break;
            }
            buffer[valread] = '\0';
            std::cout << "Recebido " << buffer << std::endl;

            // Enviar resposta
            send(new_socket, hello, strlen(hello), 0);
            std::cout << "Mensagem enviada" << std::endl;
        }

        close(new_socket);
    }

    close(server_fd);
    return 0;
}


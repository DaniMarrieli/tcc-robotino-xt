// Copyright adaptado da API Robotino v1
#define _USE_MATH_DEFINES
#include <cmath>
#include <iostream>
#include <string>
#include <cstring>
#include <vector>
#include <sys/socket.h>
#include <netinet/in.h>
#include <unistd.h>
#include <cstdio>  // Para sprintf

#include "rec/robotino/com/all.h"
#include "rec/core_lt/utils.h"
#include "rec/core_lt/Timer.h"
#include "rec/core_lt/Thread.h"

using namespace rec::robotino::com;

class MyInfo : public Info {
public:
    MyInfo() {}
    void infoReceivedEvent(const char* text) {
        std::cout << "Info: " << text << std::endl;
    }
};

class MyCom : public Com {
public:
    MyCom() {}
    void errorEvent(Error error, const char* errorString) {
        std::cerr << "Erro Com: " << errorString << std::endl;
    }
    void connectedEvent() {
        std::cout << "Conectado ao controlador." << std::endl;
    }
    void connectionClosedEvent() {
        std::cout << "Conexao fechada." << std::endl;
    }
};

class SensorManager : public Thread {
private:
    MyCom com;
    MyInfo info;
    AnalogInputArray analogInputs;  // Para IR (9 sensores)
    Battery battery;
    Bumper bumper;

public:
    SensorManager() : connected(false) {
        info.setComId(com.id());
        analogInputs.setComId(com.id());
        battery.setComId(com.id());
        bumper.setComId(com.id());
    }

    ~SensorManager() {
        if (connected) {
            com.disconnect();
        }
    }

    bool connect(const std::string& hostname) {
        std::cout << "Conectando ao controlador (" << hostname << ")..." << std::endl;
        com.setAddress(hostname.c_str());
        com.connect();
        rec::core_lt::msleep(1000);  // Aguarda conexão
        if (com.isConnected()) {
            connected = true;
            std::cout << "Conectado!" << std::endl;
            return true;
        }
        return false;
    }

    std::string readIR() {
        if (!connected) return "ERROR: Not connected";
        
        com.update();  // Atualiza dados
        std::vector<float> irValues;
        for (int i = 0; i < 9; ++i) {  // 9 IR sensors no Robotino XT
            irValues.push_back(analogInputs[i]);  // Valor analógico (0-5V)
        }
        
        std::string result = "IR: ";
        char buf[50];
        for (size_t j = 0; j < irValues.size(); ++j) {
            sprintf(buf, "%.2f", irValues[j]);
            result += buf;
            if (j < irValues.size() - 1) result += ",";
        }
        return result;
    }

    std::string readBattery() {
        if (!connected) return "ERROR: Not connected";
        
        com.update();
        float voltage = battery.voltage();
        float current = battery.current();
        float temperature = battery.temperature();
        // % aproximado: (voltage / 24.0) * 100 para bateria 24V (ajuste se necessário)
        int percentage = (int)((voltage / 24.0) * 100);
        
        char buf[100];
        sprintf(buf, "BATTERY: Voltage=%.2fV, Current=%.2fA, Temp=%.1fC, %%=%d", 
                voltage, current, temperature, percentage);
        return std::string(buf);
    }

    std::string readBumpers() {
        if (!connected) return "ERROR: Not connected";
        
        com.update();
        unsigned char bumperState = bumper.state();  // 8 bits: bit i=1 se colisão no bumper i
        
        std::string result = "BUMPER: ";
        for (int i = 0; i < 8; ++i) {
            bool state = (bumperState & (1 << i)) != 0;
            result += (state ? "1" : "0");
            if (i < 7) result += ",";
        }
        return result;
    }

    void run() {
        // Thread roda em background para manter conexão
        while (connected) {
            com.update();
            rec::core_lt::msleep(50);  // Atualiza a 20Hz
        }
    }

private:
    bool connected;
};

int main() {
    SensorManager manager;
    if (!manager.connect("172.26.1.1")) {  // IP do controlador; mude para "localhost" se necessário
        std::cerr << "Falha na conexao ao controlador. Verifique IP/firmware." << std::endl;
        return 1;
    }
    manager.start();  // Inicia thread de atualização

    // Servidor TCP
    int serverSocket = socket(AF_INET, SOCK_STREAM, 0);
    if (serverSocket < 0) {
        std::cerr << "Erro ao criar socket" << std::endl;
        return 1;
    }

    struct sockaddr_in serverAddr;
    memset(&serverAddr, 0, sizeof(serverAddr));
    serverAddr.sin_family = AF_INET;
    serverAddr.sin_addr.s_addr = INADDR_ANY;
    serverAddr.sin_port = htons(12345);

    if (bind(serverSocket, (struct sockaddr*)&serverAddr, sizeof(serverAddr)) < 0) {
        std::cerr << "Erro ao bind" << std::endl;
        close(serverSocket);
        return 1;
    }

    if (listen(serverSocket, 5) < 0) {  // Aumentei backlog para múltiplas conexões
        std::cerr << "Erro ao listen" << std::endl;
        close(serverSocket);
        return 1;
    }

    std::cout << "Servidor TCP rodando na porta 12345. Aguardando comandos do Raspberry..." << std::endl;

    while (true) {
        struct sockaddr_in clientAddr;
        socklen_t clientLen = sizeof(clientAddr);
        int clientSocket = accept(serverSocket, (struct sockaddr*)&clientAddr, &clientLen);
        if (clientSocket < 0) {
            std::cerr << "Erro ao accept" << std::endl;
            continue;
        }

        char buffer[1024];
        int bytesRead = read(clientSocket, buffer, sizeof(buffer) - 1);
        if (bytesRead > 0) {
            buffer[bytesRead] = '\0';
            std::string command(buffer);
            std::cout << "Comando recebido: " << command << std::endl;

            std::string response;
            if (command.find("HELLO") != std::string::npos) {
                response = "HELLO: Servidor Robotino v1 respondendo via API!";
            } else if (command.find("IR") != std::string::npos) {
                response = manager.readIR();
            } else if (command.find("BATTERY") != std::string::npos) {
                response = manager.readBattery();
            } else if (command.find("BUMPER") != std::string::npos) {
                response = manager.readBumpers();
            } else {
                response = "ERRO: Comando desconhecido. Use: HELLO, IR, BATTERY, BUMPER";
            }

            write(clientSocket, response.c_str(), response.length());
            std::cout << "Resposta enviada: " << response << std::endl;
        }

        close(clientSocket);
    }

    close(serverSocket);
    manager.stop();
    return 0;
}
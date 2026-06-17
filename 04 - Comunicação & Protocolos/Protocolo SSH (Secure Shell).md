# Acesso Remoto via SSH

O SSH é o protocolo principal para gerenciamento do sistema operacional, tanto no AMD Geode legado quanto no novo Raspberry Pi 4.

## 1. Parâmetros de Conexão
- Devido à antiguidade do sistema operacional original (**Ubuntu 9.04**), os protocolos de troca de chaves e algoritmos de criptografia são considerados inseguros pelos clientes SSH modernos.
- Para estabelecer a conexão com o AMD Geode, é necessário habilitar manualmente esses algoritmos no comando. 
### 🔑 Comando de Conexão Obrigatório Para conectar ao Robotino XT em sua configuração original, utiliza-se: 
```
bash ssh -oKexAlgorithms=+diffie-hellman-group1-sha1 -oHostKeyAlgorithms=+ssh-rsa tcc@172.16.104.51
```
## 2. Aplicação no Projeto
- **Modo Gateway:** Utilizado para monitorar logs de rede enquanto o Raspberry Pi tentava atuar como ponte.
- **Desenvolvimento:** Permite a compilação remota, edição de arquivos de configuração (ex: `/boot/config.txt`) e reinicialização de daemons como o `controld2` sem a necessidade de periféricos (monitor/teclado) conectados ao robô.
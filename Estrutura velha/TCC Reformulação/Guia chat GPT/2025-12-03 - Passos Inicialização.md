Ótimo dia! Vamos reiniciar o processo **do zero**, de forma totalmente organizada e sequencial, para que você valide a comunicação **Raspberry ↔ MAX232 ↔ IO Board** e confirme que o daemon **controld2** está recebendo e enviando dados corretamente.

A seguir está um **procedimento completo**, dividido em etapas claras.

---

# ✔️ **ETAPA 1 – Conectar ao Raspberry via SSH**

No seu computador Ubuntu:

```bash
ssh tcc@172.16.104.51
```

Se conectar normalmente, avance.

---

# ✔️ **ETAPA 2 – Confirmar UART no Raspberry**

### 2.1 Verificar se os dispositivos foram criados

```bash
ls -l /dev/serial*
```

O resultado **esperado** é:

```
/dev/serial0 -> ttyAMA0
/dev/serial1 -> ttyS0
```

Se estiver assim, a UART está configurada corretamente.

### 2.2 Confirmar que a UART está habilitada no boot

```bash
grep uart /boot/config.txt
```

O resultado deve mostrar:

```
enable_uart=1
dtoverlay=miniuart-bt
```

---

# ✔️ **ETAPA 3 – Testar comunicação bruta com a IO Board (minicom)**

**A IO Board deve estar alimentada pela bateria.**

### 3.1 Abrir o minicom:

```bash
sudo minicom -b 12340 -o -D /dev/serial0
```

O baudrate **OBRIGATÓRIO** do Robotino é **12340 baud**.

### 3.2 Agora pressione Enter duas vezes.

Você deve ver algo como:

```
EA09>
```

ou algum byte aleatório vindo da IO Board.

### 3.3 Teste de requisição de versão

Digite:

```
version
```

E pressione ENTER.

O **resultado esperado** é algo como:

```
EA09 version 0x0F 0xXX
```

Se NÃO aparecer resposta:

- TX/RX invertidos?
    
- GND comum realmente ligado?
    
- MAX232 recebendo 5V?
    
- Cabos firmes no conector da IO Board?
    

---

# ✔️ **ETAPA 4 – Parar qualquer daemon que esteja usando a serial**

Antes de continuar, pare o `controld2` para liberar a UART:

```bash
sudo systemctl stop controld2
```

E pare todos os daemons:

```bash
sudo systemctl stop controld3
sudo systemctl stop controld2
sudo systemctl stop rpcd
```

---

# ✔️ **ETAPA 5 – Rodar o controld2 manualmente (debug)**

Esse é o passo mais importante para sabermos se a comunicação está **estável**.

Execute:

```bash
sudo /opt/robotino/bin/controld2 -v
```

O resultado **ideal**:

```
State changed to 2
State changed to 3
Connected
```

O erro que você viu ontem:

```
EA09 message length must be >= 0 and <= 255
```

pode aparecer **uma vez na inicialização**.  
Se aparecer repetidamente, ainda há ruído ou problema elétrico.

Quando o daemon parar:

```
controld2 stopped
```

isso é normal, pois você está rodando manualmente.

---

# ✔️ **ETAPA 6 – Rodar via systemd (modo operacional)**

Reativar o daemon:

```bash
sudo systemctl restart controld2
sudo systemctl status controld2
```

O **estado correto** é:

```
Active: active (running)
```

Se entrar em looping (start/stop), rode:

```bash
journalctl -u controld2 -n 40
```

e me envie as últimas linhas.

---

# ✔️ **ETAPA 7 – Confirmar comunicação geral (rpcd)**

```bash
sudo systemctl restart rpcd
sudo systemctl status rpcd
```

Se “Listening on port 12080”, está ótimo.

---

# ✔️ **AGORA, ENVIE PARA MIM:**

1. Resultado de:
    

```bash
ls -l /dev/serial*
```

2. Saída ao rodar:
    

```bash
sudo minicom -b 12340 -D /dev/serial0
```

e digitar **version**

3. Resultado de:
    

```bash
sudo /opt/robotino/bin/controld2 -v
```

4. Se o daemon parar ou reiniciar:
    

```bash
journalctl -u controld2 -n 40
```

---

Quando me mandar essas informações, consigo diagnosticar com precisão onde a comunicação está falhando (se é elétrico, protocolo, baudrate, ruído, ou software). Vamos avançar!
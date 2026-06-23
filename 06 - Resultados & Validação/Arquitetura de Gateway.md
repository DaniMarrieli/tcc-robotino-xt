# Arquitetura de Gateway
## 📊 Resultados Obtidos

### Comunicação Validada

- Rede Ethernet operacional.
- Conexão TCP estabelecida.
- Troca de mensagens funcional.

### Integração Parcial

- Cliente e servidor funcionando.
- Comunicação entre Robotino e Raspberry Pi.

---

## ⚠️ Limitações Encontradas

### APIs Proprietárias

- Bibliotecas apenas para x86.

### Dependência do RTAI

- Forte acoplamento hardware/software.

### Incompatibilidade ARM

- Impossibilidade de recompilação.

### Controle

- Conexões abortadas.

### Odometria

- Leitura não estabilizada.

---

## ❌ Resultado Final 

Arquitetura Gateway descartada.

Motivos:

- Incompatibilidade das APIs.
- Dependência da arquitetura x86.
- Ausência de suporte e documentação oficial da Festo.
  
---
# Outubro/2025: Tentativa de Comunicação via Gateway

**Status:** Fracassado ❌

## Proposta Original
Utilizar o **Raspberry Pi 4** como um "tradutor" (Gateway), onde ele iniciaria a comunicação com o AMD Geode (Servidor) via rede para coletar odometria e enviar comandos.

## Problemas Encontrados
- **APIs Corrompidas:** Tentativas de utilizar a API v2 resultaram em erros de download e bibliotecas inconsistentes.
- **Latência e Arquitetura:** A latência de rede entre o RPi4 e o hardware legado 32-bit (AMD Geode) impediu o controle estável necessário para robótica móvel.
- **Incompatibilidade:** As bibliotecas nativas (`.so`) compiladas para Intel 80386 não eram reconhecidas pelo ambiente moderno do Raspberry Pi.

## Conclusão
A arquitetura híbrida via rede foi descartada em favor do **Retrofit Total da UCP**.
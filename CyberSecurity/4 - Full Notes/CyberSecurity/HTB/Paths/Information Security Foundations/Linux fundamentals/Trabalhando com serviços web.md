2025-12-05
Tags: [[HTB]]

----
# Conceito Geral:

### 1. PhP (O "Nativo" e Rápido)

É o favorito em CTFs porque o PHP já vem instalado na maioria das distros de segurança (Parrot/Kali) e não requer instalação extra.

- **Pré-requisito:** Estar na pasta onde estão seus arquivos.
    
- **Comando:**
    
    Bash
    
    ```
    php -S 0.0.0.0:8000
    ```
    
- **Detalhes:**
    
    - `-S`: Ativa o modo servidor.
        
    - `0.0.0.0`: **Crucial.** Permite que o servidor seja acessado por qualquer IP (incluindo a VPN do HTB), não apenas localmente.
        
    - `:8000`: A porta (pode ser qualquer uma livre).
        

---

### 2. NPM (O Moderno via Node.js)

Ideal se você prefere ferramentas baseadas em Node ou precisa de recursos como controle de cache e CORS.

- **Pré-requisito:** Ter `npm` instalado (`sudo apt install npm`).
    
- **Opção A (Sem instalar nada - Recomendado):**
    
    Bash
    
    ```
    npx http-server -p 8000
    ```
    
- **Opção B (Instalado globalmente):**
    
    1. Instale uma vez: `sudo npm install -g http-server`
        
    2. Executar:`http-server -p 8000`
        
- **Dica:** Adicione `-c-1` para desativar o cache (útil se você edita o arquivo frequentemente).
    

---

### 3. Apache2 (O Serviço de Sistema)

Diferente dos anteriores, este é um **serviço robusto** que roda em segundo plano. É usado quando você precisa de um servidor permanente ou recursos avançados (como `.htaccess`).

- Passo 1: Colocar os arquivos no lugar certo
    
    O Apache não serve a pasta onde você está. Ele serve uma pasta específica do sistema. Você deve copiar seus arquivos para lá:
    
    Bash
    
    ```
    sudo cp meu_arquivo.txt /var/www/html/
    ```
    
- Passo 2: Iniciar o Serviço
    
    Use o systemctl para ligar o servidor.
    
    Bash
    
    ```
    sudo systemctl start apache2
    ```
    
- **Passo 3: Parar (quando terminar)**
    
    Bash
    
    ```
    sudo systemctl stop apache2
    ```
    

---

### Tabela de Decisão Rápida

|**Método**|**Quando usar?**|**Vantagem**|**Desvantagem**|
|---|---|---|---|
|**PHP**|**90% dos casos em CTF.** Transferência rápida de arquivos.|Já vem instalado, comando curto.|Executa arquivos .php (pode ser perigoso se não for a intenção).|
|**NPM**|Quando precisa de log colorido ou desativar cache facilmente.|Interface limpa, opções de CORS.|Requer instalação do Node/NPM.|
|**Apache2**|Quando precisa de persistência ou simular um servidor real.|Robusto, roda em background.|Mais trabalhoso (precisa mover arquivos e usar sudo).|

-----
# Referências:



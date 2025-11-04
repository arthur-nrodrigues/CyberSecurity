2025-11-03
Tags: [[HTB]]

----
# Conceito Geral:

#### **1. O que são Serviços (Daemons)?**

- **Definição:** São programas que rodam "em segundo plano" (background) para executar tarefas cruciais do sistema, sem interação direta do usuário.
    
- **Identificação:** Seus nomes frequentemente terminam com a letra `d` (ex: `sshd`, `systemd`).
    
- **Tipos de Serviços:**
    
    - **Serviços de Sistema:** Essenciais para o funcionamento básico do sistema (como o motor de um carro).
        
    - **Serviços Instalados pelo Usuário:** Adicionam funcionalidades extras (como o GPS ou ar-condicionado de um carro).
        

#### **2. Processos**

- **PID:** Todo processo em execução no Linux recebe um **ID de Processo (PID)** único.
    
- **PPID:** Um processo pode ter um **ID de Processo Pai (PPID)**, indicando que foi iniciado por outro processo.
    
- **Estados de um Processo:**
    
    - Correndo (Running)
        
    - Aguardando (Waiting)
        
    - Parado (Stopped)
        
    - Zumbi (Zombie)
        
- **`/proc/`**: É um diretório no sistema que contém informações detalhadas sobre cada processo em execução.
    

#### **3. `systemd` e `systemctl` (Gerenciamento Moderno)**

- **`systemd`**: É o **gerente de serviços** da maioria das distribuições Linux modernas. É o primeiro processo a ser iniciado (PID 1) e gerencia todos os outros.
    
- **`systemctl`**: É o **comando principal** (o "controle remoto") que usamos para interagir com o `systemd`.
    

**Comandos Essenciais do `systemctl`:**

- **`systemctl start [serviço]`**: Inicia um serviço.
    
- **`systemctl status [serviço]`**: Verifica o estado detalhado de um serviço (se está ativo, falhou, etc.).
    
- **`systemctl enable [serviço]`**: Habilita um serviço para iniciar automaticamente com o sistema (no boot).
    
- **`systemctl list-units --type=service`**: Lista todos os serviços ativos no sistema.
    
- **`journalctl -u [serviço]`**: Usado para visualizar os logs detalhados de um serviço, muito útil para diagnosticar falhas.
    

#### **4. Gerenciamento de Processos (Sinais e `kill`)**

- **Sinais:** São a forma como o sistema se comunica com os processos (para pedir que parem, reiniciem, etc.).
    
- **`kill -l`**: Lista todos os sinais disponíveis.
    
- **`kill <PID>`**: Envia um sinal para um ID de processo específico.
    

**Sinais Mais Comuns:**

- **`SIGINT (2)`**: Interrupção. Enviado ao pressionar `[Ctrl] + C`.
    
- **`SIGKILL (9)`**: "Matar". Força o encerramento imediato do processo (não pode ser ignorado).
    
- **`SIGTERM (15)`**: "Terminar". É o sinal padrão do `kill`. Pede educadamente para o processo encerrar.
    
- **`SIGSTOP (19)`**: Para (pausa) a execução do processo.
    
- **`SIGTSTP (20)`**: Suspensão. Enviado ao pressionar `[Ctrl] + Z`.
    

#### **5. Gerenciamento de Tarefas (Jobs) no Terminal**

- **`[Ctrl] + Z`**: Suspende o processo atual e o coloca em segundo plano (envia `SIGTSTP`).
    
- **`jobs`**: Lista todos os processos suspensos ou rodando em segundo plano na sua sessão atual.
    
- **`bg <ID>`**: Coloca um processo suspenso (`Stopped`) para continuar executando em **b**ack**g**round.
    
- **`fg <ID>`**: Traz um processo do background de volta para o **f**ore**g**round (primeiro plano), permitindo interagir com ele.
    
- **Comando `&`**: Adicionar um `&` no final de um comando o inicia diretamente em background.
    
    - Ex: `ping -c 10 www.hackthebox.eu &`
        

#### **6. Executando Múltiplos Comandos**

Existem três separadores principais para executar comandos em sequência:

- **Ponto e Vírgula (`;`)**
    
    - **Função:** Executa os comandos em sequência, **independentemente do sucesso ou falha** do comando anterior.
        
    - Ex: `echo '1'; ls MISSING_FILE; echo '3'` (O erro do `ls` não impede o `echo '3'` de rodar).
        
- **E Comercial Duplo (`&&`)**
    
    - **Função:** Executa o próximo comando **apenas se** o comando anterior foi bem-sucedido (retornou 0).
        
    - Ex: `echo '1' && ls MISSING_FILE && echo '3'` (O processo para no erro do `ls` e o `echo '3'` **não** é executado).
        
- **Pipe (`|`)**
    
    - **Função:** Pega a **saída** do primeiro comando e a usa como **entrada** para o segundo comando.

-----
# Referências:


2025-12-23
Tags: [[HTB]]

----
# Conceito Geral:

# 🛡️ Resumo Técnico: Kerberoasting

### 1. O que é?

O Kerberoasting é uma técnica de pós-exploração que abusa do funcionamento legítimo do protocolo Kerberos no Active Directory (AD).

O objetivo do atacante é extrair hashes de senhas de contas de serviço para tentar quebrá-las (crack) offline e obter acesso privilegiado.

### 2. O Conceito Chave: SPN (Service Principal Name)

Para que o Kerberoasting funcione, o alvo deve ser uma conta de usuário configurada como uma conta de serviço. No AD, essas contas possuem um identificador chamado **SPN**.

- O SPN diz ao sistema: _"Esta conta `svc_sql` é responsável por rodar o serviço `MSSQLSvc/servidor.empresa.local`"_.
    

### 3. A Vulnerabilidade (Feature vs. Flaw)

O "pulo do gato" do ataque reside na arquitetura do Kerberos:

1. **Qualquer usuário autenticado** no domínio (mesmo sem privilégios administrativos) pode solicitar um ticket de serviço (TGS - Ticket Granting Service) para qualquer SPN.
    
2. O Controlador de Domínio (DC) **não verifica** se o usuário tem permissão para acessar aquele serviço específico antes de emitir o ticket.
    
3. **O ponto crítico:** Para que a conta de serviço possa descriptografar e validar o ticket, o DC criptografa uma parte desse ticket usando o **hash da senha da própria conta de serviço** (geralmente NTLM ou RC4).
    

### 4. O Fluxo do Ataque (Passo a Passo)

1. **Enumeração:** O atacante varre o AD procurando contas de usuário que possuem a propriedade `servicePrincipalName` definida.
    
2. **Solicitação (Request):** O atacante solicita um ticket TGS para esses SPNs identificados.
    
3. **Extração:** O DC responde enviando o ticket. O atacante captura esse ticket da memória (usando ferramentas como _Rubeus_ ou _Mimikatz_).
    
4. **Cracking Offline:** O atacante leva o ticket para sua própria máquina (fora da rede da vítima). Como parte do ticket está criptografada com a senha do usuário de serviço, ele usa ferramentas de força bruta (como _Hashcat_ ou _John the Ripper_) para tentar adivinhar a senha que descriptografa aquele ticket.
    

### 5. Por que é perigoso?

- **Baixo Risco de Detecção:** O cracking é feito offline. Não há bloqueio de conta por tentativas falhas de login no AD durante o processo de quebra de senha.
    
- **Privilégios:** Contas de serviço muitas vezes são membros de grupos poderosos (como _Domain Admins_) porque administradores preguiçosos dão permissão excessiva para "garantir que o serviço rode".
    
- **Senhas Fracas:** Muitos administradores configuram contas de serviço com senhas humanas e fracas, tornando o cracking trivial.
    

### 6. Ferramentas Comuns

- **Rubeus:** A "faca suíça" moderna para Kerberos (você viu nos logs do seu Sherlock).
    
- **Impacket (GetUserSPNs.py):** Muito usada em ataques via Linux/Kali.
    
- **PowerView / Invoke-Kerberoast:** Scripts em PowerShell (método mais antigo, mas ainda funcional).
    
- **Hashcat:** Para quebrar a senha (Modo 13100).
    

### 7. Defesa e Mitigação (Blue Team)

Como você é estudante de Ciência da Computação e gosta de defesa, isso é crucial:

- **Senhas Fortes:** A única defesa real contra o cracking é garantir que contas de serviço tenham senhas longas (25+ caracteres) e aleatórias.
    
- **Criptografia AES:** Forçar o uso de criptografia AES para Kerberos (em vez de RC4), o que torna o processo de cracking muito mais lento.
    
- **Monitoramento:** Alerta no Event ID **4769** (Ticket de Serviço Solicitado). Se um único usuário solicitar tickets para dezenas de serviços diferentes em segundos, é um forte indicador de Kerberoasting.
    

-----
# Referências:


2025-12-22
Tags: [[HTB]]

----
# Conceito Geral:

Basicamente o **SPF (Sender Policy Framework)** é basicamente a lista de presença de um e-mail, é nela que é verificado se um e-mail pode ou não receber um outro e-mail.

### Anatomia de um Registro SPF

Como estudante de Ciência da Computação, você precisa saber ler o código. O SPF é um registro de texto (TXT) no DNS.

Exemplo de um registro SPF real: `v=spf1 ip4:192.168.0.1 include:_spf.google.com -all`

Vamos dissecar:

- **`v=spf1`**: Versão do protocolo (sempre começa assim).
    
- **`ip4:192.168.0.1`**: "Este endereço IP específico está autorizado".
    
- **`include:_spf.google.com`**: "Se o e-mail vier dos servidores do Google, também está autorizado" (usado quando você usa Gmail corporativo).
    
- **`-all` (O mais importante):** Significa **Hard Fail**. "Se não for nenhum dos IPs acima, PROÍBA e rejeite o e-mail".
    
    - _Variação:_ `~all` (til) significa **Soft Fail**. "Se não for dos IPs acima, aceite, mas marque como suspeito/spam". Hackers adoram empresas que usam `~all` por preguiça de configurar direito.

-----
# Referências:


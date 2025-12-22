2025-12-22
Tags: [[HTB]]

----
# Conceito Geral:

### 1. O Conceito Fundamental: A Impressão Digital Digital

Um algoritmo de Hash é uma função matemática que pega um dado de _qualquer tamanho_ (uma senha de 6 letras ou um filme de 4GB) e o tritura até transformá-lo em uma string de texto de **tamanho fixo** e único.

Se o **Base64** é uma tradução e a **Criptografia** é um cofre, o **Hash** é uma **Impressão Digital**.

- Você não consegue recriar o ser humano a partir da impressão digital dele (Irreversível).
    
- Mas, se você tiver uma impressão digital encontrada na cena do crime, você pode compará-la com a do suspeito para saber se é a mesma pessoa (Verificação).
    

---

### 2. As 3 Leis do Hash (Características)

Para ser útil em segurança, um Hash precisa obedecer a estas regras:

1. **Unidirecional (One-Way):** É matematicamente impossível pegar o hash e descobrir o arquivo original.
    
    - _Exemplo:_ Se eu te der o hash de uma senha, você não consegue fazer a conta inversa para descobrir a senha. É como tentar transformar um suco de volta em uma laranja.
        
2. **Determinístico:** A mesma entrada **sempre** gera a mesma saída.
    
    - Se você fizer o hash da palavra "Arthur" hoje, amanhã ou daqui a 10 anos, o resultado será idêntico.
        
3. **Efeito Avalanche:** Se você mudar **um único bit** (uma vírgula) no arquivo original, o hash final muda completamente.
    
    - Isso é crucial para verificar se um arquivo foi alterado por um hacker.
        

---

### 3. Para que serve em Cibersegurança?

#### A. Integridade de Arquivos (Onde você vai usar muito)

Você baixou o Kali Linux ou uma ferramenta hacker. Como saber se ninguém interceptou o download e inseriu um vírus no meio do arquivo?

- O site oficial fornece o Hash original (SHA-256).
    
- Você calcula o Hash do arquivo que baixou no seu PC.
    
- Se os dois baterem, o arquivo é íntegro (perfeito). Se for diferente, foi corrompido ou alterado.
    

#### B. Armazenamento de Senhas (Onde as empresas usam)

Empresas sérias (Google, Facebook, Bancos) **nunca** salvam sua senha ("123456") no banco de dados. Se um hacker invadir e roubar o banco, ele veria as senhas de todo mundo.

- Em vez disso, eles salvam o **Hash** da senha.
    
- Quando você tenta logar, você digita "123456". O sistema calcula o hash do que você digitou e compara com o hash guardado no banco. Se bater, você entra.
    
- _É por isso que, quando você esquece a senha, o site nunca te diz qual era a antiga, ele te obriga a criar uma nova. Eles não sabem a sua senha, só o hash dela._
    

#### C. Assinatura Digital e Forense

Em uma investigação criminal, quando a polícia apreende um HD, a primeira coisa que fazem é calcular o Hash do disco inteiro. Isso garante judicialmente que "nenhuma evidência foi alterada" desde a apreensão até o julgamento.

---

### 4. Os Algoritmos Famosos (O "Zoológico" de Hashes)

Você vai encontrar esses nomes o tempo todo no Hack The Box:

- **MD5 (Message Digest 5):**
    
    - _Status:_ **QUEBRADO/OBSOLETO.**
        
    - Gera hashes de 32 caracteres. É muito rápido, mas hoje em dia é fácil criar "colisões" (dois arquivos diferentes com o mesmo hash). Ainda é usado apenas para verificar integridade de arquivos simples, nunca para senhas.
        
- **SHA-1 (Secure Hash Algorithm 1):**
    
    - _Status:_ **OBSOLETO.**
        
    - Gera hashes de 40 caracteres. O Google provou que ele não é mais seguro em 2017.
        
- **SHA-256 (Família SHA-2):**
    
    - _Status:_ **PADRÃO ATUAL.**
        
    - Gera hashes de 64 caracteres. É o que o Bitcoin usa e a maioria dos sistemas seguros hoje.
        
- **Bcrypt / Argon2:**
    
    - _Status:_ **RECOMENDADO PARA SENHAS.**
        
    - Eles são propositalmente "lentos" para calcular. Isso dificulta a vida de hackers que tentam quebrar senhas na força bruta.
        

---

### 5. Como os Hackers "Quebram" Hashes?

Se é irreversível, como o John The Ripper ou Hashcat descobrem a senha?

Eles não revertem a matemática. Eles adivinham.

1. O hacker tem o hash: `e10adc3949ba59abbe56e057f20f883e`
    
2. O hacker pega um dicionário de palavras (ex: "amor", "futebol", "123456").
    
3. Ele calcula o hash de cada palavra do dicionário.
    
4. Ele compara: "O hash de 'futebol' é igual ao hash roubado? Não. E o hash de '123456'? SIM!"
    
5. Pronto, ele descobriu que a senha é "123456".
    

Isso se chama **Ataque de Dicionário** ou **Brute Force**.

### Resumo Prático para o seu Exame/CTF:

|**Nome**|**O que é?**|**Reversível?**|**Exemplo**|
|---|---|---|---|
|**Encoding (Base64)**|Formatação de dados|**Sim** (Fácil)|`U2VuaGE=`|
|**Criptografia (AES)**|Proteção de dados|**Sim** (Com Chave)|`(Lixo ilegível)`|
|**Hash (SHA-256)**|Identidade/Integridade|**Não** (Irreversível)|`5e8848...`|

-----
# Referências:


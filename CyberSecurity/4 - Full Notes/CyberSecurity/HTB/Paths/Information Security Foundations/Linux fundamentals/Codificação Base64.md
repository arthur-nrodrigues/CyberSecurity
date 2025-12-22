
2025-12-22
Tags: [[HTB]]

----
# Conceito Geral:

Em resumo base64 é uma forma de Codificação que envolve formatar binários em texto. É usual para transformar informações entre sistemas, mas de certa forma não é seguro (Criptografado), qualquer pessoa pode facilmente "quebrar" o texto que está em base64 e obter acesso ao texto original, tornando de fato inseguro. Hackers utilizam para enviar texto entre sistemas de forma que software de detecção automática não detectem.

**FORMA DE USAR:**  

Para codificar: ``echo "MSG" | base64``
Para decodificar: ``echo "Codigo base64" | base64 -d``


-----
# Referências:


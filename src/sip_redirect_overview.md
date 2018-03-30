# Visão geral da operação

Um SIP Proxy trabalhando como [Redirect Server](https://tools.ietf.org/html/rfc3261#page-51), recebe [requisições](https://tools.ietf.org/html/rfc3261#page-27) de um [UAC](https://tools.ietf.org/html/rfc3261#page-34), processa essa requisição e [responde](https://tools.ietf.org/html/rfc3261#page-28) ["302 Moved Temporarily"](https://tools.ietf.org/html/rfc3261#page-184) para o UAC, que irá retransmitir a requisição de acordo o campo ["Contact"](https://tools.ietf.org/html/rfc3261#page-40) do [SIP Header](https://tools.ietf.org/html/rfc3261#page-29).  
Abaixo temos um fluxo de ligação SIP de um UAC efetuando uma requisição para um SIP Proxy que está operando como SIP Redirect, após a resposta do SIP Redirect, o UAC retransmite a requisição ao novo destino.
<!-- imagem draw.io + sngrep -->



# Conceito

O SIP Redirect é um modo de operação do procolo SIP com a ideia de redirecionar requisições SIP.

Redirecionamento de requisições SIP permitem que o Proxy Servers recebam requisições façam o processamento necessário e as redirecione para o destino correto, tratando como for necessário o SIP Header.

O [protocolo SIP](https://tools.ietf.org/html/rfc3261) suporta redirecionamento de [requisições](https://tools.ietf.org/html/rfc3261#page-27) e utiliza a [resposta](https://tools.ietf.org/html/rfc3261#page-28) ["302 Moved Temporarily"](https://tools.ietf.org/html/rfc3261#page-184) para indicar ao user agent client ([UAC](https://tools.ietf.org/html/rfc3261#page-34)) no campo ["Contact"](https://tools.ietf.org/html/rfc3261#page-40) do [SIP Header](https://tools.ietf.org/html/rfc3261#page-29) o(s) novo(s) endereço(s) IP que o UAC irá retransmite a requisição como novo destino.

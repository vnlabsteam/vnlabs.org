# Consulta de rota de baixo custo via Asterisk
> Os exemplos abaixo foram testadas nas versões: 11.X, 13.X

### Tech-prefix para consulta de rota de baixo custo: **98000**

Formato do número para consulta de rota de baixo custo: **98000\<numero\>**  
Exemplo de número para consulta de rota de baixo custo: **9800031983199562**  

Formato para consulta de rota de baixo custo buscando por operadora: **98100\<numero\>**  
Exemplo para consulta de rota de baixo custo buscando por operadora: **9810031983199562**  

Formato de retorno da consulta de rota de baixo custo: **\<nome_da_rota\>\#\<numero\>**  
Exemplo de retorno da consulta de rota de baixo custo: **rotaclaro#31983199562**  

### Exemplo de configuração global: sip.conf [general]
`cat /etc/asterisk/sip.conf`
```
[general]
context=default
allowoverlap=no
bindaddr=0.0.0.0
bindport=5060
nat=force_rport,comedia
```

### Exemplo de configuração de conta SIP sip.conf: [\<SIP user\>]
`cat /etc/asterisk/sip.conf`
```
[<SIP user>]
codec=gsm
host=sip.routecall.io
port=5666
fromdomain=sip.routecall.io
defaultuser=<SIP user>
fromuser=<SIP user>
secret=<SIP pass>
context=lcr-from-routecall
type=friend
qualify=yes
canreinvite=no
```

### Exemplo de contexto (dialplan): extensions.conf [from-routecall]

`cat /etc/asterisk/extensions.conf`

```
; Consulta de rota de baixo custo via SIP Redirect
[lcr-routecall]
exten => _X.,1,Verbose(3,Contexto=${CONTEXT}; EXTEN=${EXTEN}; CALLERID(name)=${CALLERID(all)};)
same  =>     n,Dial(SIP/9800000${EXTEN}@cafcfa82cf8dd05d92b7,3)
same  =>     n,Hangup()

; Recebimento da consulta de baixo custo via SIP Redirect
[lcr-from-routecall]
exten => _.,1,Verbose(3,Contexto=${CONTEXT}; EXTEN=${EXTEN}; CALLERID(name)=${CALLERID(all)};)
same  =>     n,Set(ROTA=${REGEX("^[a-z0-9]+#" ${EXTEN})})
same  =>     n,Set(NUMERO=${CUT(${EXTEN},#,2)})

same  =>     n(rn1),Goto(${ROTA},${NUMERO},1)
same  =>     n,Hangup()

same  =>     n(error),Verbose(3,CODIGO DE ERRO=${EXTEN:0:3}, FAVOR VERIFICAR https://docs.routecall.io/#errors-de-consulta)
same  =>     n,Hangup()

; Contextos de saida para cada rota
[rotavivo]
exten => _X.,1,Verbose(3,Contexto=${CONTEXT}; EXTEN=${EXTEN}; CALLERID(name)=${CALLERID(all)};)
same  =>     n,Dial(SIP/${EXTEN}@rotavivo,40,r)
same  =>     n,Hangup()

; Contextos de saida para cada rota
[rotatim]
exten => _X.,1,Verbose(3,Contexto=${CONTEXT} EXTEN=${EXTEN} CALLERID(name)=${CALLERID(all)})
same  =>     n,Dial(SIP/${EXTEN}@rotatim,40,r)
; ...
```


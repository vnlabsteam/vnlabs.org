# Consultar portabilidade via Asterisk
> Os exemplos abaixo foram testadas nas versões: 11.X, 13.X

### Tech-prefix para consulta de portabilidade númerica: **99000**

Formato do número para consulta de portabilidade númerica: **99000\<numero\>**  
Exemplo de número para consulta de portabilidade númerica: **9900031983199562**  

Formato de retorno da consulta de portabilidade númerica: **\<rn1\>\#\<numero\>**  
Exemplo de retorno da consulta de portabilidade númerica: **55321#31983199562**

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
context=from-routecall
type=friend
qualify=yes
canreinvite=no
```

### Exemplo de contexto (dialplan): extensions.conf [from-routecall]

`cat /etc/asterisk/extensions.conf`

```
; Consulta de portabilidade via SIP Redirect
[portabilidade-routecall]
exten => _X.,1,Verbose(3,Contexto=${CONTEXT}; EXTEN=${EXTEN}; CALLERID(name)=${CALLERID(all)};)
same  =>     n,Dial(SIP/${EXTEN}@<SIP user>,3)
same  =>     n,Hangup()

; Recebimento da consulta de portabilidade via SIP Redirect
[from-routecall]
exten => _X.,1,Verbose(3,Contexto=${CONTEXT}; EXTEN=${EXTEN}; CALLERID(name)=${CALLERID(all)};)
same  =>     n,GotoIf($[${REGEX("^55[0-9]{3}#" ${EXTEN:0:6})}]?rn1:error)

same  =>     n(rn1),Goto(${EXTEN:0:5},${EXTEN:6},1)
same  =>     n,Hangup()

same  =>     n(error),Verbose(3,CODIGO DE ERRO=${EXTEN:0:3}, FAVOR VERIFICAR https://docs.routecall.io/error_table.html)
same  =>     n,Hangup()

; Rotas de saida para cada operadora
[55323]
exten => _X.,1,Verbose(3,Contexto=${CONTEXT}, EXTEN=${EXTEN}, CALLERID(name)=${CALLERID(all)})
same  =>     n,Dial(SIP/${EXTEN}@rotavivo,40,r)
same  =>     n,Hangup()

[55341]
exten => _X.,1,Verbose(3,Contexto=${CONTEXT}, EXTEN=${EXTEN}, CALLERID(name)=${CALLERID(all)})
same  =>     n,Dial(SIP/${EXTEN}@rotatim,40,r)
same  =>     n,Hangup()

[55321]
exten => _X.,1,Verbose(3,Contexto=${CONTEXT}, EXTEN=${EXTEN}, CALLERID(name)=${CALLERID(all)})
same  =>     n,Dial(SIP/${EXTEN}@rotaclaro,40,r)
same  =>     n,Hangup()

[55331]
exten => _X.,1,Verbose(3,Contexto=${CONTEXT}, EXTEN=${EXTEN}, CALLERID(name)=${CALLERID(all)})
same  =>     n,Dial(SIP/${EXTEN}@rotaoi,40,r)
same  =>     n,Hangup()
; ...
```

> Teste de ligação via Asterisk CLI

```shell
hostname*CLI> channel originate Local/990000031983199562@portabilidade-routecall extension
    -- Called 990000031983199562@portabilidade-routecall
    -- Executing [990000031983199562@portabilidade-routecall:1] Verbose("Local/990000031983199562@portabilidade-routecall-00000028;2", "3,Contexto=portabilidade-routecall") in new stack
    -- Contexto=portabilidade-routecall
    -- Executing [990000031983199562@portabilidade-routecall:2] Dial("Local/990000031983199562@portabilidade-routecall-00000028;2", "SIP/990000031983199562@<SIP user>") in new stack
    -- Called SIP/990000031983199562@cafcfa82cf8dd05d92b7
    -- Got SIP response 302 "moved_temporarily" back from 138.197.71.139:5666
    -- Now forwarding Local/990000031983199562@portabilidade-routecall-00000028;2 to 'Local/55321#31983199562@from-routecall' (thanks to SIP/cafcfa82cf8dd05d92b7-00000014)
[Mar 17 02:11:21] NOTICE[101062][C-00000014]: app_dial.c:1000 void do_forward(struct chanlist *, struct cause_args *, struct ast_flags64 *, int, int, int *, struct ast_party_id *, struct ast_party_id *): Not accepting call completion offers from call-forward recipient Local/55321#31983199562@from-routecall-00000029;1
    -- Executing [55321#31983199562@from-routecall:1] Verbose("Local/55321#31983199562@from-routecall-00000029;2", "3,Contexto=from-routecall") in new stack
    -- Contexto=from-routecall
    -- Executing [55321#31983199562@from-routecall:2] GotoIf("Local/55321#31983199562@from-routecall-00000029;2", "1?rn1:error") in new stack
    -- Goto (from-routecall,55321#31983199562,3)
    -- Executing [55321#31983199562@from-routecall:3] Goto("Local/55321#31983199562@from-routecall-00000029;2", "55321,31983199562,1") in new stack
    -- Goto (55321,31983199562,1)
    -- Executing [31983199562@55321:1] Verbose("Local/55321#31983199562@from-routecall-00000029;2", "3,Contexto=55321") in new stack
    -- Contexto=55321
    -- Executing [31983199562@55321:2] Dial("Local/55321#31983199562@from-routecall-00000029;2", "SIP/31983199562@rotaclaro,40,r") in new stack
```

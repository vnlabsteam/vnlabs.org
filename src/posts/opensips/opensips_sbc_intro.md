# SBC com OpenSIPS 1/6 - Introdução

Session Border Crontroller, mais conhecido como SBC, é um software responsável
por implementar uma série requisitos relacionados ao controle de borda de rede
de protocolos VoIP, como SIP e RTP.
Por incrível que pareça, há varias definições no ambiente corporativo para SBC,
em nosso caso, iremos seguir a definição técnica e mais clara que encontramos
sobre Session Border Controller, a [RFC 5853](https://tools.ietf.org/html/rfc5853).

Também sobre a [RFC 5853](https://tools.ietf.org/html/rfc5853), iremos
seguir a sua ordem de requisitos e desta forma iremos dividir o post em 6 partes,
da seguinte forma:

1. Introdução
2. Ocultação de Topologia (Topology hiding)
3. Controle de Tráfego de Mídia (Media Traffic Management)
4. Travesia de NAT com o Protocolo SIP (Maintaining SIP-Related NAT Bindings)
5. Controle de Acesso (Access Control)
6. Criptografia e Controle de Tráfego (Media Encryption and Traffic Control)

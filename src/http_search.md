# Consultar portabilidade via API REST

### Requisição HTTP

`GET https://api.routecall.io/search`

### Parametros 

Nome | Valor | Descrição
--------- | ------- | -----------
access_token | API_KEY | Chave de API REST
number | DDDNUMBER | DDD + Número a ser consultado
search_type | match ou rn1 | Modo de busca do LCR padrão (match)

### Exemplo

```bash
curl -X GET "https://api.routecall.io/search?number=DDDNUMBER&access_token=API_KEY"
```
> A resposta da consulta retorna um JSON no formato abaixo:

```json
    {
        "number":31992167624,
        "carrier":55323,
        "in_carrier_since":"2014-12-03",
        "found":1,
        "took":16.775226,
        "lcr_name": "ROTA TELECOM",
    }
```

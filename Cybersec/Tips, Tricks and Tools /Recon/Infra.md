# Information Gathering - Infra
## Tips
- Comece pelo mais simples
## Tricks
?
## Tools
### WHOIS
O [WHOIS](https://www.techtudo.com.br/noticias/noticia/2015/03/o-que-e-whois.html), cujo nome vem da expressão de língua inglesa “who is” (quem é), é um mecanismo que registra domínios, IPs e sistemas autônomos na Internet e que serve para identificar o proprietário de um site. Alimentado por companhias de hospedagem, ele reúne todas as informações pertencentes a uma página, seja ela atrelada, no Brasil, a um CNPJ ou a um CPF. Devido sua falta de padronização, o WHOIS vem sendo substituído pelo RDAP.

Tanto a [IANA quanto as demais instituições de controle](https://www.notion.so/IANA-9a2b7ba09ed3452181aa2dab2342df53?pvs=21) possuem em suas páginas um local para pesquisa via WHOIS.
É possível também realizar as consultas via terminal:
```bash
whois dominio.com #ou IP
whois IP | grep "inetnum" #traz o bloco de IPS
whois IP | egrep "inetnum|aut-num" #traz o blco de IPS e o ASN ao qual faz parte
```
Script em Phyton com WHOIS:
```python
#!/usr/share/python

import socket,sys #importa as libs
s=socket.socket(socket.AF_INET, socket.SOCK_STREAM) #abre conexão e atrbui a s
s.connect(('whois.iana.org',43)) #passa os parâmetros para a conexão (site: whois da IANA e porta 43 (whois))
s.send(sys.argv[1]+"\r\n") #pega como argumento o site que será mandado na query da consulta
resposta = s.recv(1024.split() #guarda o retorno numa var resposta e splita em campos de separação com virgula
whois = resposta[19] #manda para a var whois o conteúdo do campo 19, que no caso é a instituição regional a quem pertence o domínio ou IP e que deve ser consultada

s1=socket.socket(socket.AF_INET, socket.SOCK_STREAM) #abre conexão e atrbui a s
s1.connect((whois,43)) #passa os parâmetros para a conexão (o whois da instituição regional que foi pego anteriormente e porta 43 (whois))
s1.send(sys.argv[1]+"\r\n") #pega como argumento o site que será mandado na query da consulta
resp = s1.recv(1024)  #guarda o retorno numa var resp
print resp #printa a resposta
```
OBS: Algumas empresas escodem seu IP atrás do serviço de alguma outra, ou utilizando um Proxy, por isso é importante sempre observar o NetName, e ver se ele corresponde ao da empresa buscada. Caso não, é importante buscar outros subdomínios da empresa e buscar por eles.\
### RDAP
O RDAP nasce como um substituto do WHOIS, visando resolver um dos seus principais problemas: a falta de padronização em seus retornos. O RDAP entrega um JSON com os dados ordenados de forma padrão.
https://client.rdap.org/
https://ftp.registro.br/pub/gts/gts21/01-RDAP.pdf
https://rdap-web.lacnic.net/
https://www.apnic.net/about-apnic/whois_search/about/rdap/
https://registro.br/rdap/
### BGP - Border Gateway Protocol
O Border Gateway Protocol (BGP), ou protocolo de rotador de borda, como pode ser traduzido para o português, é um protocolo de roteamento externo responsável por conectar a redes que formam a Internet. É um protocolo que agrega tudo e usa o TCP como o protocolo de transporte, na porta 179.
O BGP pode ajudar a mapear como os ASNs estão conectados e trazer informações de borda sobre os domínios.
https://bgpview.io/
https://bgp.he.net/
### SHODAN
[Shodan](https://www.shodan.io/) é um mecanismo de busca que permite aos usuários pesquisar vários tipos de servidores conectados à Internet usando uma variedade de filtros. Alguns também o descreveram como um mecanismo de busca de banners de serviço, que são metadados que o servidor envia de volta ao cliente. No Shodan é possível encontrar desde webcans até tanques de combustível, basta que o dispositivo esteja conectado à internet.\
https://beta.shodan.io/search/filters - Filtros de pesquisa do Shodan\
**IMPORTANTE**: O Shodan é pago, mas 1 vez por ano ele abre uma promoção a $1 ou $5 de acesso vitalício.
### Censys
Por um longo tempo, o Shodan foi o único mecanismo de busca para IoT. Em 2013, o serviço gratuito chamado [Censys](https://censys.io/ipv4?q=) surgiu para mudar esse cenário (sem as taxas do Shodan). Ele também é um motor de pesquisa para IoT com os mesmos princípios básicos mas, segundo seu criador, é mais preciso quando se trata de buscar falhas de segurança. Ah, sim, o Censys pode de fato disponibilizar uma lista dos dispositivos com uma vulnerabilidade em particular, por exemplo, aqueles suscetíveis à Heartbleed. O Censys foi criado por um grupo de cientistas da Universidade de Michigan, como instrumento para tornar a Internet mais segura. Na verdade, tanto o Shodan quanto o Censys foram feitos para pesquisas de segurança, mas à medida que recebem mais atenção, há com certeza mais gente que poderia tentar usá-los para atividades maliciosas.

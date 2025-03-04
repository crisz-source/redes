# Entendendo sobre roteamento, dns e o uso de IPV6

## Roteamento interno 

- O roteamento é o processo de encameamento e definição das rotas de qual pacotes de dados que vão utilizar nas nossas redes.
- Neste projeto, utilizei exatamente o o mesmo projeto de vlans e acrescentei mais um roteador, switch e um servidor para que poder configurar o provedor de internet
- no roteador ISP-1 A configurei da seguinte forma:
![alt text](./img-roteamento-image.png)


- ISP-1 B 
![alt text](./img-roteamento-image-4.png)

- Servidor ISP
![alt text](./img-roteamento-image-2.png)

- configurando o roteador dhcp para conectar na subrede do roteador que esta conectando o servidor e atriubiar a interface que é a serial
![alt text](./img-roteamento-image-3.png)


### Protocolo RIP 
- O protocolo Routing information Porotocol (RIP) Ele basicamente atua da seguinte forma, os roteadores vão compartilhar informações de roteamento entre si, informando quais rotas cada um dos roteadores conhece. Sendo assim, o roteador A vai saber quais redes o roteador B conhece e com isso vai poder decidir se vai encaminhar um pacote para o roteador B ou não.

- Configurando roteador para usar o protocolo RIP
- Roteador A ISP-1A
![alt text](./img-roteamento-image-5.png)

- No roteador B eu repliquei essas mesmas configurações e adicionei ip diferente
![alt text](./img-roteamento-image-6.png)

- Com os roteadores configurados para usar o protocolo RIP, ainda possui uma limitação pois o protocolo RIP na hora de um pc enviar informação a um outro pc que esta em um roteador diferente,  o protocolo RIP é "acionado" e faz a contagem de quandos roteadores está no no caminho para entrar a informação a um outro pc, então ele faz saltos. Ex: " um computador conectado ao roteador A que precisa enviar informações para um computador conectado ao roteador C, e esses roteadores estão interligados por meio do roteador B, a rota terá 2 saltos. Isso porque o pacote de dados precisa primeiro ir do computador para o roteador A, depois do roteador A para o roteador B, e finalmente do roteador B para o roteador C, onde está o computador de destino. Portanto, são 2 saltos no total.  " E esse protocolo é normalmente usado em redes mentor e não leva consideração a latência e velocidade, apenas  o  número de roteadores. E para resover essa limitação de considerar a latencia e velocidade, é recomendado utilizar o protocolo OSPF.

- Para utilizar o protocolo OSPEF, eu configurei alguns roteadores, switches e servidores, abaixo tem a explicação de passo a passo feito pela Alura.
```bash 
Acessamos a aba CLI do roteador ISP 1 - B, entramos no modo de configuração e habilitamos a interface serial 0/1/0 com os comandos interface serial 0/1/0 e no shutdown. Então, atribuímos o endereço IP 160.1.1.1 para a interface usando ip address 160.1.1.1 255.255.255.252.

Dessa vez, acessamos a aba CLI do roteador ISP 2 - C, inserimos no para configurá-lo no modo dialog, entramos no modo de configuração e habilitamos a interface serial 0/1/0 com os comandos interface serial 0/1/0 e no shutdown. Usando a mesma sub-rede 1, atribuímos um endereço IP para a interface com ip address 160.1.1.2 255.255.255.252.

Ainda no roteador ISP 2 - C, no modo de configuração, saímos da interface serial com o comando exit e acessamos a interface FastEthernet 0/0 com interface Fa0/0 (conexão entre os roteadores ISP 2 - C e ISP 2 - D). Habilitamos a interface com no shutdown* e atribuímos um endereço IP da sub-rede 2 com ip address 170.1.1.1 255.255.255.252.

Na sequência, saímos da interface FastEthernet 0/0 com exit e acessamos a interface FastEthernet 0/1 com interface Fa0/1 (conexão entre ISP 2 - C, Switch ISP 2 A e Server ISP 2 A). Habilitamos a interface com no shutdown e atribuímos um endereço IP da sub-rede 3 com ip address 180.1.1.1 255.255.255.252.

No Server ISP 2 A, clicamos na aba Desktop e selecionamos a opção IP Configuration. Então, inserimos o endereço IP 180.1.1.2 no modo estático no campo IPv4, a máscara de rede 255.255.255.252 e o default gateway 180.1.1.1.

Então, acessamos a aba CLI do roteador ISP 2 - D, inserimos no para configurá-lo no modo dialog, entramos no modo de configuração e habilitamos a interface FastEthernet 0/0 com os comandos interface Fa 0/0 e no shutdown (conexão entre os roteadores ISP 2 - C e ISP 2 -D). Atribuímos um endereço IP da sub-rede 2 com ip address 170.1.1.2 255.255.255.252.

Ainda no roteador ISP 2 - D, saímos da interface FastEthernet 0/0 com exit e habilitamos a interface FastEthernet 0/1 com interface Fa0/1 e no shutdown. Nesta interface, utilizamos um endereço IP da sub-rede 4 com ip address 190.1.1.1 255.255.255.252.

Por fim, no Server ISP 2 B, clicamos na aba Desktop e selecionamos a opção IP Configuration. Então, inserimos o endereço IP 190.1.1.2 no modo estático no campo IPv4, a máscara de rede 255.255.255.252 e o default gateway 190.1.1.1.

Verificamos a tabela de roteamento dos roteadores configurados na rede, entrando na aba CLI no modo enable e usando o comando show ip route. Observamos que, conforme esperado, os roteadores só possuíam informações das redes diretamente conectadas em suas interfaces
```

- Configurando o OSPEF no roteador ISP 2 - C
![alt text](./img-roteamento-image-7.png)

Configurando o OSPEF no roteador ISP 2 - D
![alt text](./img-roteamento-image-8.png)

- O OSPF (Open Shortest Path First) é um protocolo de roteamento que pode se adaptar facilmente ao crescimento da rede, oferecendo escalabilidade, convergência rápida e adaptabilidade às mudanças na topologia.



# Roteamento Externo

- BGP: Border Gateway Protocol, este protocolo vai atuar no encaminhamente das inforamções em redes diferentes. O protocolo OSPEF como ele é utilizado mais em redes pequenas, para que essa rede consiga fazer uma comunicação com redes externas é necessário configurar os roteadores utiliznado o protocolo BGP.

- Configurando o roteador ISP1 - B
![alt text](./img-roteamento-image-9.png)

- Repetindo o processo no C
![alt text](./img-roteamento-image-10.png)


- Verificando se deu tudo certo utilizando o BGP no roteador ISP1 - B
![alt text](./img-roteamento-image-11.png)

- Retorno se deu tudo certo: 
```bash
     150.1.0.0/30 is subnetted, 3 subnets
R       150.1.1.0 [120/1] via 150.1.1.5, 00:00:21, FastEthernet0/0
C       150.1.1.4 is directly connected, FastEthernet0/0
C       150.1.1.8 is directly connected, FastEthernet0/1
     160.1.0.0/30 is subnetted, 1 subnets
C       160.1.1.0 is directly connected, Serial0/1/0
     170.1.0.0/30 is subnetted, 1 subnets
B       170.1.1.0 [20/0] via 160.1.1.2, 00:00:00
     180.1.0.0/30 is subnetted, 1 subnets
B       180.1.1.0 [20/0] via 160.1.1.2, 00:00:00
     190.1.0.0/30 is subnetted, 1 subnets
B       190.1.1.0 [20/0] via 160.1.1.2, 00:00:00

```

- B = BGP

- Divulgando as redes pelo BGP no roteador ISP1 - B
![alt text](./img-roteamento-image-12.png)

- Verificando se esta chegando as redes no roteador ISP2 - C
![alt text](./img-roteamento-image-13.png)


# DNS 
- No roteador principal (Routeadior-DHCP)  configurei ele para que os pacotes seja enviados das maquinas para os servidores de provedores de serviços
![alt text](./img-roteamento-image-14.png)

- Realizando um teste de conexão com os servidores web:
![alt text](./img-roteamento-image-15.png)

- Configurando um servidor DNS
![alt text](./img-roteamento-image-16.png)

- Configurando as vlans para enxergar o servidor DNS e distrubir para as máquinas. (Vai ser necessário nos Pcs e fazer uma nova requisição no dhcp)
 ![alt text](./img-roteamento-image-17.png)
![alt text](./img-roteamento-image-18.png)

- Testando o DNS
![alt text](./img-roteamento-image-19.png)

# IPV6
- No IPv6, existe um endereçamento de 128 bits divididos em 8 intervalos de 16 bits cada. Para o IPv6 teríamos o exemplo de **2001:0db8:85a3:0000:0000:8a2e:0370:7334**. Outro ponto importante é que a representação dos endereços IP do IPv4 é decimal, utilizando números de 0 a 9. 
No IPv6, utilizamos o sistema de representação hexadecimal, que mistura letras e números. Além disso, no IPv6 não usa mais a máscara de rede como era feito no IPv4. Utiliza-se uma nomenclatura que é o IPv4, mas que é mais comumente utilizada no IPv6, que é a nomenclatura CIDR (Classless Inter-Domain Routing), que basicamente representa a máscara de rede com o número de bits usados, por exemplo, para identificar um host na rede. Seria uma barra e o número de bits dedicados a isso.
- Se pegarmos um endereço como **2001:0db8:85a3:0000:0000:8a2e:0370:7334**, esses grupos de quatro zeros podem ser retirados do nosso endereço. Então, removemos esses grupos de zeros, ficando :: no seu lugar:
**2001:0db8:85a3::8a2e:0370:7334** 
- Um ponto importante a se destacar é que isso só pode ser utilizado uma única vez, outra simplificação que pode ser feito é remover os zeros iniciais de cada intervalo, a fim de simplificar a representação.

- Em um endereço ipv6: 2002:BAA::BBBB/64 ele significa que esse endereço possui uma identificação de rede/sub-rede por conta de /64, onde indicia que os 64bits primeiros é essa identificação de rede/sburede e o restante identicação dos hosts
- 2003:BAA → Faz parte do prefixo da rede. Este prefixo (os primeiros 64 bits) identifica a rede/sub-rede à qual os dispositivos pertencem.
- ::AAAA → É a parte usada para identificar o host dentro da rede. Neste caso, está no espaço dos últimos 64 bits.


### Configurando o IPv6 
- Foi adicionado um novo servidor no projeto que vai suportar apenas o IPV6 Padrão, e lembrando que os endereços de IP IPV6 não existe as classes A, B e C.
![alt text](./img-roteamento-image-20.png)

- Com isso feito, vai ser necessário configurar o switch para que esse servidor seja vinculado a vlan30, que é uma vlan apenas para servidores
![alt text](./img-roteamento-image-21.png)

- Atribuindo um endereço IP para o servidor
![alt text](./img-roteamento-image-22.png)

- Configurando o roteador dhcp e atribuir para a interface que esta conectada o servidor IPv6 o endereço ip IPv6 e atribuir o protocolo Ipv6 ao roteador
![alt text](./img-roteamento-image-23.png)

- Adicionando um ipv6 a um pc 
![alt text](./img-roteamento-image-24.png)

- Relizando o teste de conectitividade
![alt text](./img-roteamento-image-25.png)


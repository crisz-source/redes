# Entendendo sobre roteamento, dns e o uso de IPV6

## Roteamento interno 

- O roteamento é o processo de encameamento e definição das rotas de qual pacotes de dados que vão utilizar nas nossas redes.
- Neste projeto, utilizei exatamente o o mesmo projeto de vlans e acrescentei mais um roteador, switch e um servidor para que poder configurar o provedor de internet
- no roteador ISP-1 A configurei da seguinte forma:
![alt text](image.png)


- ISP-1 B 
![alt text](image-4.png)

- Servidor ISP
![alt text](image-2.png)

- configurando o roteador dhcp para conectar na subrede do roteador que esta conectando o servidor e atriubiar a interface que é a serial
![alt text](image-3.png)


### Protocolo RIP 
- O protocolo Routing information Porotocol (RIP) Ele basicamente atua da seguinte forma, os roteadores vão compartilhar informações de roteamento entre si, informando quais rotas cada um dos roteadores conhece. Sendo assim, o roteador A vai saber quais redes o roteador B conhece e com isso vai poder decidir se vai encaminhar um pacote para o roteador B ou não.

- Configurando roteador para usar o protocolo RIP
- Roteador A ISP-1A
![alt text](image-5.png)

- No roteador B eu repliquei essas mesmas configurações e adicionei ip diferente
![alt text](image-6.png)

- Com os roteadores configurados para usar o protocolo RIP, ainda possui uma limitação pois o protocolo RIP na hora de um pc enviar informação a um outro pc que esta em um roteador diferente,  o protocolo RIP é "acionado" e faz a contagem de quandos roteadores está no no caminho para entrar a informação a um outro pc, então ele faz saltos. Ex: " um computador conectado ao roteador A que precisa enviar informações para um computador conectado ao roteador C, e esses roteadores estão interligados por meio do roteador B, a rota terá 2 saltos. Isso porque o pacote de dados precisa primeiro ir do computador para o roteador A, depois do roteador A para o roteador B, e finalmente do roteador B para o roteador C, onde está o computador de destino. Portanto, são 2 saltos no total.  " E esse protocolo é normalmente usado em redes mentor e não leva consideração a latência e velocidade, apenas  o  número de roteadores. E para resover essa limitação de considerar a latencia e velocidade, é recomendado utilizar o protocolo OSPF.

- OSPEF
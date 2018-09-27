###  Projeto: Reprodução do Artigo Jellyfish: Networking Data Center Randomly 

Fonte: http://pbg.cs.illinois.edu/papers/jellyfish-nsdi12.pdf

Código: 

### O artigo 

O problema de expansão de data center de forma incremental para atender os requisitos de um data center apresenta diversos desafios que ao longo do tempo diversos trabalhos vêm tentando resolver, já que a expansão desses data centers precisam atender requisitos como, largura de banda, aumento do número de servidores que crescem frequentemente. O aumento de servidores se dá pela grande demanda de diversos novos serviços. Assim,  diversas soluções abordam o problema para redes de alta capacidade, entretanto poucas consideram a questão do  problema da expansão incremental de data centers de forma a preservar as propriedade estruturais ou seja sem a substituição de elementos da rede. A principal topologia empregada era a de Fat tree, que são altamente estrutura dificultando a expansão incremental de data center. Proposta como a Scafida e SWCD são similares a ideia do Jellyfish já que empregam a aleatoriedade, mas necessitam de correlação entre as aresta de ligação.   

### Jellyfish 

Apresenta uma abordagem que visa criar uma topologia não estruturada, aleatória, de alta capacidade, permitindo ainda uma fácil expansão incremental. Jellyfish adiciona links aleatoriamente entre os switches para formar um gráfico aleatório regular. Esse método permite que switches e hosts sejam adicionados individualmente para formar redes de tamanho arbitrário. Assim diferente dos trabalhos da literatura a maior contribuição do Jellyfish está na utilização de grafos aleatórios para permitir uma maior expansão e na quantificação dos ganhos de eficiência que os grafos trazem sobre as topologias utilizadas em data center. 

Assim os diversos experimentos foram rodados, sendo possível constatar que, além de permitir a expansão incremental, a Jellyfish é mais eficiente em termos de custos do que o Fat tree, permitindo a utilização de 25% mais servidores usando os mesmo equipamentos de comutação. Foi constatado também que topologias Jellyfish tem o mesmo ou maior rendimento por servidor do que as topologias  Fat tree comparáveis a comprimento de caminho médio menor. O resultados depende do uso de roteamento K-shortest em vez do ECMP, já que foi constatado que o ECMP não oferece diversidade de caminhos suficiente para o Jellyfish. 

***

### Reprodução do resultados

O capítulo 5 do artigo apresenta a avaliação de roteamento e controle de congestionamento. Assim como parte do trabalho foi solicitado a reprodução de parte do experimento, figura 9 e tabela 1. Para realização dos testes os autores utilizaram um simulador desenvolvido pelos autores do MPTCP. 

### Figura 9 

Apresenta os resultados dos testes realizados com Equal-cost multi-path routing (ECMP) com 8 e 64 vias e K-shortest paths (kSP). Os autores argumentam que o ECMP de 64 vias não tem o desempenho muito melhor que o ECMP de 8 vias quando comparados. Já o KPS utilizado para realizar os teste tem maior benefício para o Jellyfish, já que pode utilizar caminhos mais longos, para isso é utilizados um algoritmo sem loops, e é definido K = 8 caminhos. Os resultados podem ser observados na figura abaixo.  

![](https://github.com/carlosapepe/TP1/blob/master/Figura%209.png)

### Tabela 1 

Com o objetivos de demonstrar a eficiência da Jellyfish os autores realizaram um comparativo entre as topologias Fat tree e Jellyfish. Os resultados da utilização do controle de congestionamento utilizando TPC padrão com 1 e 8 fluxo por servidores e também com o MPTCP usando o valor recomendado de 8 sub fluxos. Assim os resultados mostram a taxa de transferência média normalizada por servidor como uma porcentagem taxa de NIC dos servidores. 

![](https://github.com/carlosapepe/TP1/blob/master/Tabela1.png)

### Implementação

Para a implementação foi realizada uma pesquisa buscando encontrar  repositórios que pudessem conter o código, assim foram encontrados alguns no github:

* https://github.com/lechengfan/cs244-assignment2
* https://github.com/lechengfan/CC244-assignment2
* https://github.com/aghalayini/CS244_jellyfish

A partir de repositórios foi possível realizar testes buscando replicar o experimento da figura 9 e da tabela 1.
Para iniciar os testes foram preciso instalar algumas ferramentas como: 

1. Virtual box: https://www.virtualbox.org/wiki/Downloads
1. Ubuntu server 16.04.5 LTS: https://www.ubuntu.com/download/alternative-downloads#alternate-ubuntu-server-installer
1. Simulador Mininet: http://mininet.org/download/
1. Pox: https://github.com/noxrepo/pox
1. Linguagem python : https://www.python.org/download/releases/2.7.2/

Bibliotecas necessárias: 
* graphviz: software para visualização de grafos (Não obrigatório) 
* networkx: Pacote para criação, manipulação e estudo de estruturas dinâmicas. Utilizado para criação de grafos.
* netaddress: Possibilita criar, manipular e operar endereços de rede IP.

A partir desse conjunto de ferramentas instaladas foi possível dar início aos testes, para gerar a figura 9 e Tabela 1. Para reprodução da Figura foi utilizado uma biblioteca Networkx.  Para criação de rotas são utilizados os protocolos ECMP e KPS que utilizam as funções all_shortest_paths e shortest_simple_paths. Depois de contar os links a ferramenta matplotlib.pyplot plota o gráfico. 

Para executar a figura 9 utilize os seguintes comandos: 

* Abra o terminal e digite:
```
cd TP1/pox/ext
```
```
sudo python ./graph.py
```
Deis de Como pode ser observado nos testes realizados a reprodução da figura 9 ficou muito semelhante à que foi desenvolvida pelos autores

![](https://github.com/carlosapepe/TP1/blob/master/fig9.svg)


Inicialmente para reprodução da tabela 1 foram utilizados os mesmo parâmetros escolhidos pelos autores, entretanto com a limitação de recursos computacionais encontrada foi necessários fazer uma adaptação para chegar em um resultado.  Assim, para possibilitar a realização dos testes  foram definidos alguns padrões como pode ser observado na tabela a abaixo. 

| Controle de Congestionamento | Fat-tree (686 svrs)ECMP  | Jellyfish (10 svrs)  ECMP | Jellyfish (100 svrs)  8-shortest paths | Jellyfish (30 svrs)  ECMP | Jellyfish (30 svrs) 8- Shortest paths |
|:----------------------------:|:------------------------:|:-------------------------:|:--------------------------------------:|:-------------------------:|---------------------------------------|
|          TCP 1 flow          |                          |           48,10%          |                 49,1 %                 |           12,11%          |                 45,44%                |
|          TPC 4 Flow          |                          |                           |                 85,37 %                |          87,03 %          |                 62,82%                |
|          TCP 8 flow          |                          |                           |                 65,70 %                |          52,45 %          |                52,45 %                |
|       MPTCP 8 subflows       |                          |                           |                                        |                           |                                       |

### Dificuldade 

Dificuldade de encontrar um código de criação de topologia Fat tree que funcionasse 

Dificuldade com o MPTCP - implementação e geração dos resultados. Esse problema dificultou o sucesso do experimento  de modo geral, já que o trabalho original utiliza o MPTCP. 
Problemas para gerar os resultados da tabela com MPTCP  de 8 subfluxos 
Problemas com o Iperf 










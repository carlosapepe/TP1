
## Como instalar o codigo

Certifique-se de que python2 e pip2 estejam instalados.
Execute o “pip2 install matplotlib”
Execute “pip2 install networkx”
Execute "git clone git: //github.com/mininet/mininet"
Execute "git checkout -b 2.2.1 2.2.1"
Execute "mininet / util / install.sh -a"
Usando o comando “ls”, você deve ver vários novos diretórios.
Execute o "git clone https://github.com/aghalayini/CS244_jellyfish.git" (Alterar o para o meu caminho
Execute o  cd "CS244_jellyfish / pox / ext /"
Execute o "python2 ./graph.py"
*Você deverá ver dois arquivos .svg em seu diretório atual. Eles são a forma do seu gráfico aleatório e reproduzem a figura 9 do artigo. 
 Execute o "sudo python2 ./build_topology.py"
*A coleta de dados ocorrerá antes do comando mininet CLI (net), que dá ao usuário acesso ao prompt de comando mininet. 
 Os resultados do Iperf em cada host serão gerados em um arquivo h.out no mesmo diretório / ext, junto com um arquivo h.err no caso de haver algum erro.
*Você também verá uma taxa de transferência média na saída impressa.
*Você pode alterar os parâmetros em 
# parameters seção # de build_topology.py para alterar o método de roteamento (por exemplo, "ecmp8"), o número de conexões TCP e o tamanho da rede.
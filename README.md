# Sistemas-a-Eventos-Discretos-PROJETO-02
Repositório para o Projeto 02 da disciplina Sistemas a Eventos Discretos 2024.1 UFCG
Equipe: Adeilson, Jackson e Tobias.

## Sistema de transporte de carga

A seguir será apresentado um sistema de transporte de carga consiste em uma via circular com 8 secções e 3 veículos. Entre cada uma das secções existe um semáforo que controla a entrada dos veículos na secção. Exceto entre as secções 2 e 3.

Por questões de segurança, apenas um veículo pode transitar em uma seção de cada vez.

Inicialmente os veículos se encontram parados em um estacionamento. A partir desse estacionamento, os veículos acessam a seção 1 e entram na via de transporte de cargas.

O tempo de deslocamento entre cada uma das seções é de 10 unidades de tempo. Em cada mudança de seção o veículo deve parar para carregar/descarregar. Essa operação dura pelo menos 25 unidades de tempo.

O veículo n (n = 1, 2 ou 3) para para reabastecer a cada n voltas na via. Para reabastecer ele entra no estacionamento pelo acesso que existe na seção 1.

O modelo consistem em dois Templates desenvolvidos na ferramenta UPPAAL:
  * **Template 1**: Veiculo (Figura 1) - apresenta as seções em que o veículo pode se encontrar (8 seções + estacionamento), nesse template é mostrado as guardas que fazem o papel de       semáforo, observando o caso em que o estado _S_n (n = 1, 2, 4, 5, 6, 7, 8)_ está vazio (_S_n == 0_). Caso esse estado esteja vazio, o veiculo efetua a transição de seção e atualiza os valores dos estados anterior e atual, aterando seus valores para 0 (passa a estar desocupado) e 1 (passa a estar ocupado) respecivamente. 
Cada transição é sincronizada por um canal 'Pronto', que é acionado pelo segundo autômato, que modela as relações temporais, respeitando as especificações de tempo apresentadas no enunciado.
Cada Veiculo recebe um indice (_n = 1, 2 ou 3_) que é utilizado para verificar a autonomia dos veículos, de modo que é feita uma contagem de voltas e cada um deve parar para reabastecer a cada n voltas.

_Figura 1: Modelo para os veiculos._
![image](https://github.com/user-attachments/assets/6e2ab9a3-7036-40a3-aab9-39c21ae022e8)

* **Template 2**: Estados (Figura 2) - Neste automato é apresentado os estados que o veículo pode estar: I) Se deslocando e II) Pronto para carregamento.
No estado Deslocamento, o veiculo aguarda Td == 10 unidades de tempo para completar a transição e assim mudar para o estado 'Pronto para carregamento', nesse estado, o veiculo aguarda um Tc >= 25 unidades de tempo para completar o carregamento e retornar para o estado 'Deslocamento'. Nessa transição é enviado o sinal 'Pronto!' para habilitar a transição de seções.

_Figura 2: Modelo para os estados._
![image](https://github.com/user-attachments/assets/3583d52d-00d1-4d76-afe0-68f7f5075cad)

Para teste e verificação do modelo, as expressões da figura 3 foram verificadas: 

_Figura 3: Testes para o modelo._
![image](https://github.com/user-attachments/assets/ae4d1685-7cd0-4127-a78a-f92235017d10)

A primeira query:  A[] !(Veiculo1.secao == Veiculo2.secao && Veiculo1.secao != 0), testa se Em todos os estados futuros do sistema, os veículos 1 e 2 não devem estão na mesma seção, exceto na seção 0 - Estacionamento. De modo semelhante o teste é aplicado para as seções subsequentes (2-3 e 1-3).

Na quarta query: A[] Veiculo1.voltas <= 1, é verificado que o Veículo 1 nunca completa mais de 1 volta ao longo de toda a execução do sistema, respeitando sua autonomia de 1 volta. De igual modo para os veículos 2 e 3.

Por fim a condição de não haver deadlock vou verificada: A[] not deadlock, sendo a mesma satisfeita.

Por fim conclui-se que o modelo atende aos requesitos especificados, onde foi possivel modelar um sistema de transporte com 3 veículos e 8 seções com logica temporal. Pode-se verificar a importancia da utilização da ferramenta UPPAAL para a excução do modelo para simular os 3 veículos e as relações temporais.

[Aqui é apresentado o vídeo de demonstração.](https://youtu.be/pp5NXqMd9fI) 



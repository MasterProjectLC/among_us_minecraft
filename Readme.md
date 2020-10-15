# Among Us no Minecraft - Uma conversão de um jogo 2D para um ambiente 3D

## Conceito do projeto
Este projeto trata-se de uma conversão das mecânicas e sistemas do popular jogo de dedução social "Among Us" para o ambiente do jogo sandbox "Minecraft". Para isto, muitas técnicas e implementações comuns à linguagens de programação tiveram de ser abstraídas, simuladas e traduzidas para o sistema nativo de "blocos de comando" que o jogo possui.

Para um resumo das regras, sistemas e mecânicas de Among Us, vide [Guia de Among Us](https://steamcommunity.com/sharedfiles/filedetails/?id=1908555816) para um guia em português e [A Long Guide Among Us](https://steamcommunity.com/sharedfiles/filedetails/?id=2220357997) para uma explicação mais completa (porém em inglês).

Para uma explicação sobre o jogo Minecraft, [esta página na Minecraft Wiki](https://minecraft-pt.gamepedia.com/Minecraft) é recomendada.
 
## Pré-requisitos e recursos utilizados
O grupo utilizou o videogame Minecraft[1] para o desenvolvimento principal, com o jogo Among Us[2] e o site Minecraft Wiki[3] como principais recursos de consulta.

Outros recursos utilizados foram:
*WebFx[4] para a obtenção e conversão de cores no jogo;
*Gamewith[5] para um melhor detalhamento do sistema de Tarefas do jogo Among Us;
*PlanetMinecraft[6] para mais informações sobre o sistema de "Bossbars" de Minecraft.

  
## Passo a passo
1. O servidor-ambiente de jogo foi criado
2. O mapa do jogo foi examinado, simulado e construído no ambiente de jogo
3. O setup inicial ("código" executado para inicializar o jogo) foi iniciado, assim como o ambiente de desenvolvimento
4. O sistema de "cooldown" foi implementado pela primeira vez
5. O sistema de votação foi implementado
6. O sistema de escotilhas foi implementado e construído
7. O sistema de morte foi implementado
8. O sistema de configurações do jogo (setup antes do jogo) foi implementado
9. O sistema de condições de vitória foi implementado
10. Os quatro sistemas de Sabotagem (Nuclear, Elétrica, O2 e Comms) foram implementados
11. O gerenciador de Tarefas, criado para distribuir e controlar o sistema de Tarefas no início e decorrer do jogo, foi implementado
11. Os treze sistemas de Tarefa (Admin, Divert, Upload, Medscan, Manifolds, Sample, Shields, Align Output, Steering, Fuel, Garbage, Asteroids e Wiring) foram implementados
12. O sistema de Escuta, criado para substituir o sistema de Câmeras no jogo original, foi criado e implementado
13. O quinto sistema de Sabotagem (Portas) foi implementado
14. O sistema de Compasso, criado para ajudar a orientar os jogadores pelo mapa, foi implementado
15. O sistema de cosméticos, criado para maior customização dos jogadores por meio de "máscaras", foi implementado

Tais passos foram intercalados por diversos bugfixes e testes coletivos. Destaque ao sistema de votação, que de longe foi o sistema com mais problemas durante o desenvolvimento do projeto.

## Instalação
Acesse o servidor oficial do Hackoonspace de Minecraft (versão atual - 1.16.3), e siga para o lobby de Among Us sinalizado na área inicial.

## Execução
1. Reúna 5-12 jogadores no lobby do jogo.
2. Pressione o botão de configurações para modificar certas regras do jogo.
3. Pressione o botão no centro da sala para iniciar o jogo. 

A maioria das regras segue idêntica ao jogo original. Todas as diferenças estão listadas sob o botão "Regras".

## Implementação
A implementação foi feita principalmente pelo uso de "blocos de comando", que podem ser comparados a linhas de código em linguagens de programação convencionais. Foi feito também uso de "redstone", o sistema elétrico do jogo, para a implementação de portas lógicas, certos loops e, de certo modo, funções.

![Imagem](https://github.com/MasterProjectLC/among_us_minecraft/blob/master/Redstone2.png)
Redstone utilizada para formar a porta lógica "AND".

### Comandos principais
Dezenas de tipos de comandos foram utilizados, mas alguns merecem destaque por sua utilidade e similaridade a estruturas de programação:
* Execute: similar a um "if"; também usável para a atribuição de certas variáveis.
* Scoreboard: o sistema de scoreboard foi essencial para o jogo, sendo responsável quase inteiramente por gerenciar as variáveis e as constantes do jogo, assim como certas formas de input e operações lógicas envolvendo elas.
* Tellraw: com o uso deste comando, foi possível utilizar o formato JSON do jogo para formar mensagens personalizadas e imprimí-las para jogadores específicos na área de "chat" do jogo. Em essência, serviu como tanto o "printf" quanto o "scanf" do jogo.

### Estruturas de blocos de comando
No decorrer do projeto, várias técnicas foram descobertas para facilitar a implementação de certas funcionalidades do projeto por meio de diferentes arranjos de combinações de blocos com comandos.

#### A função
A estrutura mais básica e mais essencial do projeto foi, sem dúvida, a função. Apesar de Minecraft oferecer seu próprio sistema de funções (com o commando /function), este necessita a instalação de datapacks. Por isso, um sistema mais rústico foi utilizado, formado por um comando "chamador" da função, que posiciona um bloco ativador na frente dos blocos de comando, e os blocos de comando em si, que formam a função.

![Imagem](https://github.com/MasterProjectLC/among_us_minecraft/blob/master/Function.png)
Comando chamador da função.

![Imagem](https://github.com/MasterProjectLC/among_us_minecraft/blob/master/Function2.png)
Função.

#### O do/while
Para criar a mecânidade de votação, loops tiveram de ser criados para se adaptar à quantidade variável de jogadores no jogo. Assim, um sistema semelhante à um loop "do/while" foi criado, formado pela função principal, um bloco-condição, que checa uma condição usando execute e chama blocos de comandos ajudantes que invocam a função principal novamente após uma curta espera, e um bloco-quebra, que é chamado ao fim do loop.

![Imagem](https://github.com/MasterProjectLC/among_us_minecraft/blob/master/Looper.png)
Estrutura marcada com placas para sinalização.

#### O checador
Minecraft fornece uma opção de blocos de comando contínuos, que são invocados a todo tick. Minecraft também oferece a opção de blocos condicionais, que são chamados apenas quando o anterior for bem-sucedido. Apesar de muito úteis, estes, quando pareados, às vezes não são suficentes, pois em uma situação de "switch/case", ou todos os comandos são executados a todo tick (assim gastando processamento), ou nenhum é chamado. Para evitar este problema, a estrutura "checador" foi criada, que troca os blocos condicionais por um único bloco contínuo com o comando "execute", que, quando bem-sucedido, chama uma função com o resto dos comandos.

![Imagem](https://github.com/MasterProjectLC/among_us_minecraft/blob/master/Checker.png)
O checador, sinalizado com placas.

#### A cadeia de conjuntos de comando
Para o sistema de distribuição de tarefas, foi necessário sortear um jogador aleatório para a obtenção de cada Tarefa do jogo, repetindo a função até que todos os jogadores tiverem a quantidade necessária de Tarefas. A obtenção de tarefas para um jogador, porém, necessita que diversos comandos sejam executados em um mesmo jogador sorteado aleatoriamente.
Para criar isto, a cadeia de comandos foi criada. A cada "nódulo" da cadeia, um jogador aleatório recebe uma tag "Target" para receber a Tarefa do nódulo seguinte, enquanto que o jogador com a tag do nódulo anterior recebe a sua própria tarefa. Assim, os comandos rodam de forma eficiente e facilmente expandível - para adicionar uma nova tarefa ao jogo, é necessário apenas criar um novo nódulo.

![Imagem](https://github.com/MasterProjectLC/among_us_minecraft/blob/master/Nodes.png)
A cadeia de conjuntos de comando.

## Bugs/problemas conhecidos
Devido à diferença intrínseca de ambos ambientes e às limitações do projeto, algumas mecânicas do jogo original tiveram de ser retiradas ou pesadamente modificadas para se adaptar ao ambiente 3D. Estas incluem:
* Fantasmas (jogadores mortos) não podem realizar Tarefas. Ao invés disso, Tarefas de fantasmas são completas a cada 20 segundos, a partir do momento de suas mortes. Esta funcionalidade foi retirada devido aos diversos problemas causados pela presença e influência física dos jogadores mortos no jogo.
* O sistema de câmeras foi substituído por um sistema de escuta que transmite "pings" de jogadores em certos pontos do mapa para a sala de segurança. A substituição foi feita com relutância, pois o sistema de câmeras seria tecnicamente possível: apesar de Minecraft não suportar a criação de câmeras em tempo-real, um sistema alternativo utilizando invisibilidade, teletransporte e o uso de uma criatura na posição original do jogador[7] possibilitaria a implementação de tal mecânica. Decidimos, porém, que tal funcionalidade seria esdrúxula demais para o design da simulação, e retiramos o sistema.

## Autores
* Danilo Isamu Inafuku ([MasterProject](https://github.com/MasterProjectLC)) - Programação e planejamento
* Marcus Vinícius Natrielli Garcia  - Hosteamento e testes
* Fernando Favareto Abromovick - Construção e testes
* Michel Ribeiro Koba - Construção e testes
* Gustavo de Jesus Rodrigues Silva - Construção e testes
* Vinícius Venturini - Construção
* Caio Brandini - Construção e testes
* Mauricio Cândido de Souza - Planejamento e testes
* Felipe Bertoni Salvati - Testes

## Referências/Bibliografia
[1] Site oficial de Minecraft. Disponível em: https://www.minecraft.net/pt-pt
[2] Site oficial de Among Us. Disponível em: http://www.innersloth.com/gameAmongUs.php
[3] Minecraft Wiki. Disponível em: https://minecraft.gamepedia.com/Minecraft_Wiki
[4] WebFX. Hex to RGB. Disponível em: https://www.webfx.com/web-design/hex-to-rgb/
[5] Gamewith. Tasks List - Common & Visual Tasks List. Disponível em: https://gamewith.net/among-us-wiki/article/show/22106
[6] ShelLuser. Setting up a bossbar in Minecraft 1.13. Disponível em: https://www.planetminecraft.com/data-pack/setting-up-a-bossbar-in-minecraft-1-13/
[7] CHAU, Hamish. Among Us (1.16.3). Disponível em: http://phoenixsc.me/download-links/among-us-1-16-3/
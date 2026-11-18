# 👻 Ghost Multiplayer

Jogo multiplayer local de perseguição desenvolvido em **Jack**, a linguagem utilizada no curso [nand2tetris](https://www.nand2tetris.org/). O projeto adapta o jogo Ghost original para dois jogadores em um labirinto, com colisões, cronômetros e reinício automático.

## 🎮 Sobre o jogo

Dois jogadores compartilham o teclado:

- **Jogador 1 — Fantasma:** precisa alcançar a pessoa antes que o tempo acabe.
- **Jogador 2 — Pessoa:** precisa fugir do fantasma até o fim da contagem regressiva.

Cada rodada possui duas fases:

1. **Preparação (3 segundos):** os dois personagens aparecem no mapa, mas somente a pessoa pode se movimentar.
2. **Perseguição (20 segundos):** o fantasma é liberado e tenta capturar a pessoa.

Se os personagens ocuparem a mesma célula durante a perseguição, **o fantasma vence**. Se o tempo acabar sem captura, **a pessoa vence**. O resultado aparece na tela e uma nova rodada começa automaticamente.

## 🕹️ Controles

| Ação | Jogador 1 — Fantasma | Jogador 2 — Pessoa |
|---|---|---|
| Cima | `↑` | `W` |
| Baixo | `↓` | `S` |
| Esquerda | `←` | `A` |
| Direita | `→` | `D` |
| Parar | `.` ou `;` | `E` |
| Sair do jogo | `Esc` ou `Q` | `Esc` ou `Q` |

**Movimentação automática:** basta indicar uma direção. O personagem continua se movendo até encontrar uma parede, receber outra direção ou ser parado pelo jogador. Esse mecanismo facilita o uso do mesmo teclado por duas pessoas.

## ✅ Funcionalidades

- Dois personagens controlados por jogadores humanos.
- Labirinto fixo com paredes, corredores e limites de movimentação.
- Colisão com o cenário: os personagens não atravessam paredes.
- Sprites de 16 × 16 pixels desenhados na memória de vídeo.
- Posições iniciais pseudoaleatórias em células livres e distintas.
- Cronômetro regressivo visível para preparação e perseguição.
- Detecção de captura entre fantasma e pessoa.
- Tela inicial com instruções, tela de resultado e reinício automático.
- Encerramento por `Esc` ou `Q`.

## 🧠 Como funciona

### Mapa e colisões

A tela de **512 × 256 pixels** é organizada como uma grade de **32 colunas × 16 linhas**, com células de **16 × 16 pixels**. A classe `GameMap` armazena as células em um `Array` unidimensional:

- `0`: espaço livre;
- `1`: parede.

O índice de cada célula é calculado por `linha * 32 + coluna`. Antes de movimentar um personagem, o jogo consulta `GameMap.isWall()` para verificar se a próxima célula está bloqueada. A linha superior é reservada ao texto do cronômetro e bloqueada para movimentação.

### Tempo e fases

A classe `GameTimer` contabiliza atualizações (*ticks*) do loop de jogo. Após um número configurado de ticks, diminui o contador de segundos. O mesmo cronômetro é reiniciado ao passar da preparação para a perseguição e ao iniciar uma nova rodada.

> **Nota:** a contagem é aproximada e depende da velocidade de execução do VM Emulator; não é baseada em um relógio de tempo real.

### Sorteio das posições

A classe `Random` usa um gerador pseudoaleatório. A semente inicial depende de quanto tempo o jogador demora para pressionar uma tecla na tela de título. O método de posicionamento repete os sorteios até encontrar células livres e diferentes para os dois personagens.

### Ciclo da partida

```text
Tela inicial
    ↓
Criar mapa e posicionar jogadores
    ↓
Preparação (3 s) — só a pessoa anda
    ↓
Perseguição (20 s) — ambos andam
    ↓
Captura? ── Sim ──> Fantasma vence
    │
    Não
    ↓
Tempo acabou? ── Sim ──> Pessoa vence
    ↓
Mostrar resultado e reiniciar a rodada
```

## 📁 Estrutura do projeto

```text
Ghost/
├── Main.jack        # Ponto de entrada
├── GhostGame.jack   # Loop, fases, controles, captura e reinício
├── Ghost.jack       # Sprite e deslocamento do fantasma
├── Person.jack      # Sprite e deslocamento da pessoa
├── GameMap.jack     # Labirinto e colisões com paredes
├── GameTimer.jack   # Contagem regressiva
└── Random.jack      # Geração de números pseudoaleatórios
```

O `Main` cria o `GhostGame`, chama `run()` e libera os recursos no encerramento. O `GhostGame` coordena as demais classes por meio de métodos como `startRound()`, `playRound()`, `showGameOver()` e `endRound()`.

## ▶️ Como executar

**Pré-requisitos:** ferramentas do [nand2tetris](https://www.nand2tetris.org/software), incluindo o compilador Jack e o VM Emulator.

1. Coloque todos os arquivos `.jack` na mesma pasta, por exemplo `Ghost/`.
2. Compile **a pasta inteira** com o `JackCompiler` (no Windows, pelo `JackCompiler.bat`, usando a pasta como argumento).
3. Abra o `VMEmulator` (no Windows, `VMEmulator.bat`).
4. Em **Load Program**, selecione a pasta com os arquivos `.vm` gerados.
5. Configure a execução conforme necessário e clique em **Run**. O ponto de entrada é `Main.main()`.

> Se você alterar algum arquivo `.jack`, compile novamente e recarregue o programa no VM Emulator para testar a versão atualizada.

## 🔧 Possíveis melhorias futuras

- Seleção de dificuldade: **fácil, médio e difícil**, com três mapas diferentes.
- Ajustes de tempo e equilíbrio entre os jogadores.
- Indicador de vitórias por jogador.
- Melhorias visuais na interface e nos sprites.

Esses recursos são **ideias futuras**, não funcionalidades da versão atual.

## 🎓 Contexto acadêmico

Projeto desenvolvido para a disciplina que utiliza a plataforma **nand2tetris**, com foco em programação na linguagem Jack, manipulação de memória de vídeo, organização em classes, entrada pelo teclado, lógica de colisões e gerenciamento do ciclo de um jogo.

A implementação parte do exemplo original **Ghost** fornecido para a atividade e o amplia para o modo multiplayer.

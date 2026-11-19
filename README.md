# 🧬 Algoritmo Genético aplicado ao Flappy Bird

Projeto da disciplina **Inteligência Computacional** (Tema 3 — Computação
Evolucionária). Implementa um algoritmo genético completo aplicado ao jogo
Flappy Bird, com uma demonstração visual no navegador e um treinador offline
em C++ capaz de produzir e validar um genoma campeão.

| Informação | Detalhes |
|---|---|
| **Membros** | Ivan Guimarães — RM 0051352421038 <br> Felipe Juca — RM 0051352411031 |
| **Disciplina** | Inteligência Computacional |
| **Professor(a)** | Felipe Cannarozzo |
| **Tema** | Grupo 3 — Computação Evolucionária e Algoritmos Evolucionários |

## 1. O que tem neste repositório

Dois artefatos independentes, com papéis diferentes:

- **Uma demonstração visual no navegador** — mostra a população evoluindo em
  tempo real, com gráfico de convergência, controles de velocidade e execução
  do campeão carregado. Serve para **apresentar** o algoritmo.
- **Um treinador offline em C++** — executa o mesmo algoritmo genético sem
  interface, com sementes fixas, cenários de treino compartilhados e
  **validação independente**. Serve para **produzir e validar** o campeão de
  verdade.

A demonstração JS não treina o campeão apresentado; o campeão é gerado pelo
treinador C++ e carregado no navegador para a demonstração final.

## 2. Estrutura do repositório

```text
.
├── README.md                              ← este arquivo (apresentação + mapa)
├── assets/                                
|           
└── scripts/
    ├── explicacao_teorica/
    │   └── FASE0_MATEMATICA.ipynb         ← (fundamentos matemáticos do GA)
    └── simulacao_algoritmo/
        ├── README.md                      ← arquitetura e uso da demo JS
        ├── index.html                     ← (ponto de entrada da demo)
        ├── style.css
        ├── config.js
        ├── brain.js
        ├── game.js
        ├── evolution.js
        ├── ui.js
        ├── main.js
        └── treino_cpp/
            ├── README.md                  ← como treinar e validar o campeão
            ├── treinar.cpp
            ├── treinar.exe                ← (pré-compilado Windows)
            └── campeao.json               ← (genoma validado, v2)
```

## 3. Onde encontrar cada coisa

Este repositório tem três pontos de entrada. Use o que corresponde à sua
pergunta:

| Você quer… | Vá para | O que encontra lá |
|---|---|---|
| Entender a **matemática** do GA (genoma, sigmoide, fitness, seleção, cruzamento, mutação) | [`scripts/explicacao_teorica/FASE0_MATEMATICA.ipynb`](scripts/explicacao_teorica/FASE0_MATEMATICA.ipynb) | Notebook didático com deduções e exemplos numéricos. |
| Entender a **arquitetura da demo JS** (o que cada `.js` faz, como o loop funciona, peculiaridades da visualização) | [`scripts/simulacao_algoritmo/README.md`](scripts/simulacao_algoritmo/README.md) | Responsabilidade de cada arquivo, como executar, como ajustar, ordem de leitura. |
| **Treinar** o modelo de verdade e obter o `campeao.json` | [`scripts/simulacao_algoritmo/treino_cpp/README.md`](scripts/simulacao_algoritmo/treino_cpp/README.md) | Por que o C++ é o treinador real, parâmetros, compilação, execução, códigos de saída, formato do campeão. |

### Por que existem dois "treinadores"

O JavaScript roda dentro do navegador: `Math.random()` não é reprodutível,
não há validação independente, o treino pesado bloquearia a interface e não
há paralelismo real. Ele existe para **visualizar** o algoritmo e executar um
campeão já treinado. O treinador em C++ usa `mt19937` com sementes fixas,
avalia todos os genomas nos mesmos 32 cenários de treino e valida o melhor
candidato em 20 cenários independentes antes de declarar a meta atingida —
é o único fluxo que produz o campeão que vai para a apresentação.

Detalhes completos dessa comparação em
[`treino_cpp/README.md`](scripts/simulacao_algoritmo/treino_cpp/README.md).

## 4. Mini-roteiro para apresentação

### Ver a demonstração no navegador

Abra [`scripts/simulacao_algoritmo/index.html`](scripts/simulacao_algoritmo/index.html)
no Chrome, Firefox ou Edge. A população começa a evoluir automaticamente.

### Treinar o campeão (C++)

Pré-requisito: compilador C++17 (GCC/MinGW, Clang ou equivalente). **Na raiz do projeto**, execute o comando abaixo, ele irá entrar na pasta de treino, criar o *arquivo de simulação de treino*  em seguida, gerar o arquivo *campeao.json*, contendo os dados do pássaro já evoluído:

```powershell
Set-Location scripts\simulacao_algoritmo\treino_cpp
g++ -O2 -std=c++17 treinar.cpp -o treinar.exe
.\treinar.exe
```

O treinador cria ou substitui `campeao.json` nesta pasta. Confira no terminal
se apareceu **META ATINGIDA**; caso contrário, o JSON ainda contém o melhor
candidato medido, mas **não** deve ser apresentado como aprovado na meta de
validação de 100 canos. Código de saída `0` = meta atingida; `2` = treino
encerrado sem atingir; `1` = erro de execução.

### Apresentar o campeão

Abra `index.html` no navegador (ou o servidor acima), clique em
**Carregar campeão**, selecione `treino_cpp\campeao.json` e clique em
**Rodar campeão**.

> Para a palestra: mostre a evolução JS apenas como ilustração da população;
> o campeão apresentado é o que veio do treinador C++.

## 5. Avisos operacionais

- **Executar o treinador substitui `campeao.json`.** Se quiser preservar o
  campeão versionado, faça backup antes de rodar `.\treinar.exe`.
- **Recompilar substitui `treinar.exe`.** O binário incluído é um pré-compilado
  Windows; para garantir que corresponde ao fonte atual, recompile antes de
  usar.
- **O treino pode demorar.** Dependendo da CPU, o processo completo (até
  1.000 gerações com 80 indivíduos × 32 cenários) leva minutos a dezenas de
  minutos. É normal o terminal ficar em silêncio entre linhas de log — ele
  imprime a cada 10 gerações.
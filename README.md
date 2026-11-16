# FreteScript — Cálculo de Frete e Logística

| Integrante | RA |
| --- | --- |
| Bruno dos Reis Ferreira Santos | 2564952 |
| Vinicius de Freitas Ferreira | 2373516 |
| Matheus Francisco | 2693112 |
| Matheus de Oliveira | 2369955 |

## Sobre o projeto

Projeto da disciplina de Compiladores. A FreteScript é uma linguagem de domínio específico para simular cotações, despachos, recusas e consultas de entregas. Todas as operações são acadêmicas e locais: não há transportadora, banco de dados ou API.

## Ficha da linguagem

| Campo | Definição |
| --- | --- |
| Nome | FreteScript |
| Domínio | Cálculo de frete e logística simulada |
| Tipos | `inteiro`, `decimal`, `texto`, `logico` |
| Comandos logísticos | `cotar modalidade, peso, valor, prazo;`, `despachar codigo;`, `recusar motivo;`, `rastrear codigo;` |
| Regras de negócio | Peso maior que zero; prazo inteiro de pelo menos um dia; aviso para valor acima de R$ 1.000 |
| Programas originais | Cotação normal; despacho com rastreamento; recusa logística |

O terceiro programa utiliza um limite de peso próprio do seu cenário, sem criar uma regra universal para a linguagem.

## Tokens

O analisador Lark define 38 tipos de token e reconhece 17 palavras reservadas. Estes são os principais nomes reais usados no notebook:

| Categoria | Tokens |
| --- | --- |
| Tipos | `TIPO_INTEIRO`, `TIPO_DECIMAL`, `TIPO_TEXTO`, `TIPO_LOGICO` |
| Comandos | `SE`, `SENAO`, `ENQUANTO`, `MOSTRAR`, `COTAR`, `DESPACHAR`, `RECUSAR`, `RASTREAR` |
| Lógicos | `VERDADEIRO`, `FALSO`, `E_LOGICO`, `OU_LOGICO`, `NAO_LOGICO` |
| Valores | `IDENTIFICADOR`, `NUM_INTEIRO`, `NUM_DECIMAL`, `TEXTO` |
| Operadores | `MAIS`, `MENOS`, `MULTIPLICACAO`, `DIVISAO`, `MAIOR`, `MENOR`, `MAIOR_IGUAL`, `MENOR_IGUAL`, `IGUAL`, `DIFERENTE`, `ATRIBUICAO` |
| Delimitadores | `ABRE_CHAVE`, `FECHA_CHAVE`, `ABRE_PARENTESE`, `FECHA_PARENTESE`, `VIRGULA`, `PONTO_VIRGULA` |

Espaços e comentários `//` são ignorados; os tokens mantêm linha, coluna e lexema.

## Gramática

O parser LALR do Lark usa, entre outras, as regras abaixo. `expr` se desdobra em níveis de precedência para operações lógicas, comparações e aritmética.

```text
programa: comando*
declaracao: tipo IDENTIFICADOR (ATRIBUICAO expr)? PONTO_VIRGULA
atribuicao: IDENTIFICADOR ATRIBUICAO expr PONTO_VIRGULA
condicional: SE expr bloco (SENAO bloco)?
enquanto: ENQUANTO expr bloco
bloco: ABRE_CHAVE comando* FECHA_CHAVE
cotar: COTAR expr VIRGULA expr VIRGULA expr VIRGULA expr PONTO_VIRGULA
despachar: DESPACHAR expr PONTO_VIRGULA
recusar: RECUSAR expr PONTO_VIRGULA
rastrear: RASTREAR expr PONTO_VIRGULA
?expr: expr_ou
```

## Funcionalidades

O notebook reúne análise léxica, parser e AST, verificação semântica, geração de TAC, três técnicas de otimização, máquina virtual, testes de integração e painel interativo. Os erros mostram fase e localização; as cotações acima do limite geram avisos.

## Tecnologias

Foram utilizados Python, Jupyter Notebook/JupyterLab, Lark e ipywidgets. `nbformat`, `nbclient` e `ipykernel` apoiam a execução e validação do notebook.

## Como executar

Instale Python e execute os comandos na pasta que contém o notebook. No Windows:

```powershell
py -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m jupyterlab
```

No Linux/macOS:

```bash
python3 -m venv .venv
.venv/bin/python -m pip install -r requirements.txt
.venv/bin/python -m jupyterlab
```

Abra `FreteScript.ipynb`, escolha o kernel do ambiente virtual e use **Executar tudo**. Os testes desta auditoria foram feitos com Python 3.13 no Windows; os comandos Linux/macOS são instruções de execução, sem validação local nesta auditoria.

O GitHub mostra o notebook estaticamente. Os botões precisam de execução no JupyterLab.

## Como utilizar

Escolha um exemplo e clique em **Carregar exemplo**; o editor também aceita código próprio. **Analisar** mostra Tokens, AST, Semântica e TAC. **Executar** mostra as saídas e estatísticas da VM. **Comparar TAC** mostra as versões original e otimizada, a redução real e a equivalência observável. Há opção para executar o TAC otimizado.

## Exemplos de saída

Na cotação normal, a VM registra `Cotação Economico: 4.5 kg, R$ 38.40, prazo 3 dia(s)`. No segundo programa, o código `FS-2026-001` é despachado com status `Despachado` e rastreado duas vezes. Um `rastrear "FS-INEXISTENTE";` produz diagnóstico de execução com categoria `RASTREAMENTO_INEXISTENTE`, localização e dica. O valor é informado pelo programa; a VM não calcula tarifa adicional.

## Testes

Na validação final local, o notebook executou em kernel novo com `nbclient`, sem erros de célula. Os testes efetivamente executados foram:

| Etapa | Aprovados | Reprovados |
| --- | ---: | ---: |
| R2 — Léxico | 17 | 0 |
| R3 — Sintaxe | 25 | 0 |
| R4 — Semântica | 35 | 0 |
| R5 — TAC | 36 | 0 |
| R6 — Otimização | 36 | 0 |
| R7 — Máquina virtual | 49 | 0 |
| R8 — Integração | 25 | 0 |
| Interface Jupyter | 23 | 0 |
| **Total** | **246** | **0** |

O painel foi testado no JupyterLab com botões, abas, diagnóstico semântico, cotação e despacho com rastreamento. Não há capturas reais disponíveis nesta pasta; sua inclusão futura é opcional.

## Arquivos da entrega

- `FreteScript.ipynb` — compilador, exemplos, testes e painel.
- `README.md` — apresentação e instruções.
- `requirements.txt` — dependências.
- `.gitignore` — exclusões locais.

O material original do professor, a pasta `.venv` e os checkpoints locais não devem ser publicados. Ainda não houve commit, push ou publicação.

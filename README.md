# Analisador Sintático e Semântico MicroPascal

Este projeto consiste em um analisador sintático (baseado em um parser preditivo descendente recursivo) e semântico para a linguagem **MicroPascal**, escrito na linguagem C.

O analisador realiza a leitura de arquivos de código-fonte Pascal, processa os tokens através de um scanner (analisador léxico integrado), constrói uma Árvore Sintática Abstrata (AST) e valida as regras de tipos semânticos. Além disso, exporta a árvore gerada no formato Graphviz para visualização gráfica.

---

## Recursos Implementados

1. **Procedimento `CasaToken`**: Rotina padrão para validar e consumir tokens, interrompendo a compilação imediatamente ao detectar um token inválido.
2. **Visualização Gráfica da AST (Graphviz Digraph)**:
   - Constrói uma árvore sintática abstrata completa do fluxo do programa.
   - Gera um arquivo `ast.dot` contendo a especificação do dígrafo (Graphviz).
   - Se o comando `dot` estiver instalado no seu Mac, compila automaticamente a especificação em um diagrama gráfico de alta resolução chamado `ast.png`.
3. **Geração e Impressão de AST (Console)**: Imprime a AST de forma textual hierárquica no terminal.
4. **Trace de Regras de Produção**: Exibe no terminal a sequência de derivações/regras de produção utilizadas. Pode ser silenciado via parâmetro.
5. **Suporte a Sinais Unários**: Análise correta de expressões matemáticas com sinal de mais ou menos inicial (ex: `x := -5;`).
6. **Suporte a Underscore (`_`)**: Identificadores válidos podem conter caracteres `_` (ex: `valor_total`).
7. **Tratamento de Erros e Encerramento Imediato**:
   - O processo finaliza instantaneamente no primeiro erro (sintático, léxico ou semântico) com código de erro `1`.
   - Saídas de erro formatadas:
     - `nn:erro lexico [lex]` (caracteres inválidos ou reais malformados como `123.`)
     - `nn:token nao esperado [lex]` (erros sintáticos)
     - `nn:fim de arquivo nao esperado` (fim de arquivo inesperado)
8. **Análise Semântica Forte (Tabela de Símbolos & Tipos)**:
   - Registro de variáveis e tipos (`integer`, `real` e `boolean`).
   - Verificação de variáveis duplicadas na mesma declaração.
   - Detecção de uso de variáveis não declaradas.
   - Verificação de compatibilidade de tipos em atribuições.
   - **Tipagem Booleana Estrita**: Comparações relacionais resultam em `boolean` (`TYPE_BOOL`).
   - **Verificação de Condições**: Condicionais de `if` e loops `while` devem obrigatoriamente resultar no tipo `boolean`.
   - **Restrição de Operações**: Impede operações aritméticas e relacionais inválidas envolvendo variáveis booleanas (ex: `x := (a < b) + 5;` gera erro semântico).

---

## Como Compilar o Código

No terminal, execute o seguinte comando:

```bash
gcc -Wall -Wextra -o semantico semantico.c
```

---

## Como Executar e Visualizar

### 1. Execução Padrão (gera `ast.dot` e `ast.png` com o trace de produções)
```bash
./semantico nome_do_arquivo.pas
```

### 2. Execução Silenciosa (gera os mesmos arquivos gráficos ocultando o trace de derivações)
```bash
./semantico -s nome_do_arquivo.pas
```

Ao rodar com sucesso, serão criados dois arquivos no mesmo diretório:
- `ast.dot`: O código fonte em formato de dígrafo Graphviz.
- `ast.png`: A imagem contendo o desenho da árvore gerada. Se não for compilada automaticamente, você pode gerá-la rodando:
  ```bash
  dot -Tpng ast.dot -o ast.png
  ```

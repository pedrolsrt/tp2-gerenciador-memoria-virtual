# Trabalho Prático 2 – Gerenciador de Memória Virtual

Simulador de gerenciamento de memória virtual desenvolvido em linguagem C para a disciplina de Sistemas Operacionais da PUC Minas.

## Autor

Pedro Lucas Soares Rezende

## Disciplina

Sistemas Operacionais

Professor: Lucas Bragança da Silva

## Objetivo

O objetivo deste projeto é simular o funcionamento de um sistema de memória virtual, realizando a tradução de endereços lógicos para endereços físicos por meio de estruturas clássicas utilizadas em sistemas operacionais modernos.

A implementação contempla:

- Tabela de Páginas (Page Table);
- Translation Lookaside Buffer (TLB);
- Paginação por demanda;
- Tratamento de Page Faults;
- Substituição de páginas utilizando o algoritmo Aging;
- Coleta de estatísticas de desempenho.

---

## Especificações da Simulação

| Componente | Valor |
|------------|--------|
| Espaço de Endereçamento Virtual | 65.536 bytes |
| Número de Páginas | 256 |
| Tamanho da Página | 256 bytes |
| Número de Quadros Físicos | 128 |
| Tamanho do Quadro | 256 bytes |
| Memória Física Total | 32.768 bytes |
| Entradas do TLB | 16 |
| Política do TLB | FIFO |
| Política de Substituição de Páginas | Aging |

---

## Estrutura do Projeto

```text
tp2-gerenciador-memoria-virtual/
│
├── include/
│   ├── config.h
│   ├── memory.h
│   ├── page_table.h
│   ├── statistics.h
│   └── tlb.h
│
├── src/
│   ├── main.c
│   ├── memory.c
│   ├── page_table.c
│   ├── statistics.c
│   └── tlb.c
│
├── data/
│   ├── BACKING_STORE.bin
│   ├── addresses_random.txt
│   └── addresses_location.txt
│
├── report/
│
├── Makefile
└── README.md
```

---

## Funcionamento

O simulador recebe uma sequência de endereços lógicos como entrada.

Para cada endereço informado, o programa executa os seguintes passos:

1. Extrai o número da página e o deslocamento (offset);
2. Consulta o TLB;
3. Em caso de TLB Miss, consulta a tabela de páginas;
4. Em caso de Page Fault, carrega a página do BACKING_STORE.bin;
5. Atualiza a tabela de páginas;
6. Atualiza o TLB;
7. Calcula o endereço físico;
8. Obtém o valor armazenado na memória física;
9. Exibe os resultados da tradução.

---

## Translation Lookaside Buffer (TLB)

O TLB possui 16 entradas e é utilizado para acelerar o processo de tradução de endereços.

A política de substituição adotada foi FIFO (First In, First Out).

Quando o TLB atinge sua capacidade máxima, a entrada mais antiga é removida para dar lugar à nova tradução.

---

## Tabela de Páginas

A tabela de páginas possui 256 entradas e é responsável pelo mapeamento entre páginas virtuais e quadros físicos.

Cada entrada armazena:

- Número do quadro físico;
- Bit de validade;
- Bit de referência;
- Contador de envelhecimento (Aging Counter).

---

## Tratamento de Page Fault

Quando uma página não está carregada na memória física:

1. O simulador identifica a falta de página;
2. Localiza a página correspondente no arquivo BACKING_STORE.bin;
3. Carrega a página para um quadro disponível;
4. Atualiza a tabela de páginas;
5. Atualiza o TLB.

Caso não existam quadros livres, uma página vítima é selecionada para substituição.

---

## Algoritmo Aging

A substituição de páginas é realizada utilizando o algoritmo Aging, uma aproximação do algoritmo LRU (Least Recently Used).

Cada página mantém:

- Um bit de referência;
- Um contador de envelhecimento de 8 bits.

Periodicamente:

- O contador é deslocado para a direita;
- O bit de referência é inserido no bit mais significativo;
- O bit de referência é reiniciado.

Quando a memória física está cheia, a página com menor contador de envelhecimento é escolhida como vítima.

---

## Estatísticas

Ao final da execução são apresentadas as seguintes métricas:

- Número de endereços traduzidos;
- Quantidade de Page Faults;
- Taxa de Page Fault;
- Quantidade de TLB Hits;
- Taxa de acerto do TLB.

Essas informações permitem avaliar o comportamento do simulador e o impacto da localidade de referência sobre o desempenho do sistema.

---

## Compilação

Na raiz do projeto:

```bash
make
```

---

## Execução

Utilizando o arquivo de endereços aleatórios:

```bash
./vm < data/addresses_random.txt
```

Utilizando o arquivo com localidade de referência:

```bash
./vm < data/addresses_location.txt
```

---

## Conceitos Aplicados

- Memória Virtual
- Paginação
- Tradução de Endereços
- Tabela de Páginas
- Translation Lookaside Buffer (TLB)
- FIFO
- Page Fault
- Demand Paging
- Aging Algorithm
- Aproximação de LRU
- Gerenciamento de Memória
- Sistemas Operacionais

---

## Considerações Finais

Este projeto permitiu aplicar de forma prática conceitos fundamentais de gerenciamento de memória estudados na disciplina de Sistemas Operacionais, proporcionando uma compreensão mais aprofundada dos mecanismos utilizados pelos sistemas modernos para tradução de endereços, gerenciamento de páginas e otimização de acessos à memória.

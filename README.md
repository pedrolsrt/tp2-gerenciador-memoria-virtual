# Trabalho Prático 2 – Gerenciador de Memória Virtual

Simulador de gerenciamento de memória virtual desenvolvido em linguagem C para a disciplina de Sistemas Operacionais da PUC Minas.

## Autor

Pedro Lucas Soares Rezende

## Disciplina

Sistemas Operacionais

Professor: Lucas Bragança da Silva

## Objetivo

O objetivo deste projeto é implementar um simulador de memória virtual capaz de traduzir endereços lógicos em endereços físicos utilizando estruturas e técnicas clássicas de gerenciamento de memória presentes em sistemas operacionais modernos.

A solução implementa:

- Tabela de Páginas (Page Table);
- Translation Lookaside Buffer (TLB);
- Paginação por demanda;
- Tratamento de Page Faults;
- Substituição de páginas utilizando o algoritmo Aging;
- Coleta de estatísticas de execução.

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
| Política de Substituição | Aging |

---

## Estrutura do Projeto

```text
tp2-gerenciador-memoria-virtual/
│
├── data/
│   ├── BACKING_STORE.bin
│   ├── addresses_random.txt
│   └── addresses_location.txt
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
├── report/
│   └── relatorio_tp2_pedro_lucas_soares_rezende.pdf
│
├── Makefile
└── README.md
```

---

## Funcionamento

O simulador recebe uma sequência de endereços lógicos e executa as seguintes etapas:

1. Extrai o número da página e o deslocamento (offset);
2. Consulta o TLB;
3. Em caso de TLB Miss, consulta a tabela de páginas;
4. Em caso de Page Fault, carrega a página do BACKING_STORE.bin;
5. Atualiza a tabela de páginas;
6. Atualiza o TLB;
7. Calcula o endereço físico;
8. Lê o valor armazenado na memória física;
9. Exibe o resultado da tradução.

---

## Translation Lookaside Buffer (TLB)

O TLB foi implementado com 16 entradas e tem como objetivo reduzir a quantidade de acessos à tabela de páginas.

A política de substituição utilizada é FIFO (First In, First Out). Quando o TLB está cheio, a entrada mais antiga é substituída pela nova tradução.

---

## Tabela de Páginas

A tabela de páginas possui 256 entradas e armazena as informações necessárias para o mapeamento entre páginas virtuais e quadros físicos.

Cada entrada contém:

- Número do quadro físico;
- Bit de validade;
- Bit de referência;
- Contador de envelhecimento.

---

## Tratamento de Page Fault

Quando uma página não está presente na memória física:

1. O simulador detecta a falta de página;
2. Localiza a página correspondente no arquivo BACKING_STORE.bin;
3. Carrega a página para a memória física;
4. Atualiza a tabela de páginas;
5. Atualiza o TLB.

Caso não existam quadros livres disponíveis, é realizada a substituição de páginas utilizando o algoritmo Aging.

---

## Algoritmo Aging

A substituição de páginas foi implementada utilizando o algoritmo Aging, uma aproximação eficiente do algoritmo LRU (Least Recently Used).

Cada página mantém:

- Um bit de referência;
- Um contador de envelhecimento de 8 bits.

Periodicamente:

- O contador é deslocado para a direita;
- O bit de referência é inserido no bit mais significativo;
- O bit de referência é reiniciado.

Quando a memória física está cheia, a página com menor contador de envelhecimento é selecionada para substituição.

---

## Estatísticas

Ao final da execução, o simulador apresenta:

- Número de endereços traduzidos;
- Quantidade de Page Faults;
- Taxa de Page Fault;
- Quantidade de TLB Hits;
- Taxa de acerto do TLB.

Essas métricas permitem avaliar o comportamento da memória virtual e a eficiência da utilização do TLB.

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

Utilizando o arquivo de endereços com localidade de referência:

```bash
./vm < data/addresses_location.txt
```

---

## Relatório

O relatório técnico completo encontra-se na pasta:

```text
report/relatorio_tp2_pedro_lucas_soares_rezende.pdf
```

---

## Conceitos Aplicados

- Memória Virtual
- Paginação
- Tradução de Endereços
- Tabela de Páginas
- Translation Lookaside Buffer (TLB)
- FIFO
- Demand Paging
- Page Fault
- Aging Algorithm
- Aproximação de LRU
- Gerenciamento de Memória
- Sistemas Operacionais

---

## Considerações Finais

Este projeto permitiu aplicar na prática conceitos fundamentais de gerenciamento de memória estudados na disciplina de Sistemas Operacionais, proporcionando uma compreensão mais aprofundada dos mecanismos utilizados para tradução de endereços, paginação por demanda, substituição de páginas e otimização de acessos à memória.

A implementação integra TLB, tabela de páginas, memória física e algoritmo Aging, simulando de forma consistente o funcionamento básico de um sistema de memória virtual.

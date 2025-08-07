# S.O. 2025.1 - Atividade 2.02 - Gestão de memória

## Informações gerais

- **Objetivo do repositório**: Repositório para atividade avaliativa dos alunos
- **Assunto**: Gestão de memória
- **Público alvo**: alunos da disciplina de SO (Sistemas Operacionais) do curso de TADS (Superior em Tecnologia em Análise e Desenvolvimento de Sistemas) no CNAT-IFRN (Instituto Federal de Educação, Ciência e Tecnologia do Rio Grande do Norte - Campus Natal-Central).
- disciplina: **SO** [Sistemas Operacionais](https://github.com/sistemas-operacionais/)
- professor: [Leonardo A. Minora](https://github.com/leonardo-minora)
- Repositótio do aluno: [Alencast](https://github.com/Alencast)

## Tarefas do aluno
1. Fork desse repositório e atualizar a linha 10 com o nome e link do github
2. Ler a descrição da atividade
3. Montar a resposta no final deste arqivo, no tópico **Resposta**

---

## 1. Descrição da atividade
### 1.1. Objetivo
Praticar os conceitos de alocação de memória (best-fit), memória virtual e desfragmentação em um sistema com memória limitada.

---

### 1.2. Contexto
Um computador possui apenas **64 KB de RAM** e um **disco rígido para memória virtual**. O sistema operacional deve gerenciar 5 processos com tamanhos diferentes, cuja soma ultrapassa a capacidade da RAM.

#### 1.2.1. Processos iniciais

| Processo | Tamanho (KB) |
|----------|-------------|
| P1       | 20          |
| P2       | 15          |
| P3       | 25          |
| P4       | 10          |
| P5       | 18          |
| **Total**| **88 KB**   |

- **Memória RAM**: 64 KB (contígua, inicialmente vazia).  
- **Memória Virtual (Disco)**: Espaço ilimitado para paginação.

#### 1.2.2. Alocação Inicial com Best-Fit
Os alunos devem simular a alocação dos processos na RAM usando o algoritmo **best-fit**.  
- A memória RAM será representada como um bloco contíguo (ex: `[0KB - 64KB]`).  
- Devem alocar os processos nos menores espaços livres que atendam ao seu tamanho.  

**Alocação inicial**:  
1. P1 (20 KB) → Ocupa [0-20].  
2. P2 (15 KB) → Ocupa [20-15].  
3. _continuar a partir daqui_

#### 1.2.3. Simular Memória Virtual (Paginação)
- Os processos não alocados na RAM devem ser "paginados" no disco.  
- Criar uma tabela de páginas indicando quais partes estão na RAM e quais estão no disco.  

#### 1.2.4. Desfragmentação da RAM
- Desfragmentar a RAM para liberar espaço contíguo.
- Após desfragmentação (compactação), verificar quais processos podem ser alocado.  

### 1.3. Questões para Reflexão
1. Best-fit foi mais eficiente que first-fit ou worst-fit neste cenário?  
2. Como a memória virtual evitou um deadlock?  
3. Qual o impacto da desfragmentação no desempenho do sistema?  

---

## Resposta

### 1. Alocação Inicial Utilizando Best-Fit

#### 📘 Situação Inicial:
- **Memória RAM:** 64 KB (espaço contíguo, completamente livre no início)
- **Memória Virtual (Disco):** espaço disponível ilimitadamente
- **Processos:**

| Processo | Tamanho (KB) |
|----------|--------------|
| P1       | 20           |
| P2       | 15           |
| P3       | 25           |
| P4       | 10           |
| P5       | 18           |
| **Soma Total** | **88 KB** |

---

#### 🔁 Processo de Alocação - Etapas

1. **P1 (20 KB)**  
   - Encaixado no menor espaço disponível → início da RAM `[0–64]`.  
   - **Ocupação:** `[0–20]`.  
   - **Espaço livre restante:** `[20–64]` (44 KB).

2. **P2 (15 KB)**  
   - Alocado no menor buraco restante → `[20–64]` (44 KB).  
   - **Ocupação:** `[20–35]`.  
   - **Livre:** `[35–64]` (29 KB).

3. **P3 (25 KB)**  
   - Inserido no bloco livre `[35–64]` (29 KB).  
   - **Ocupação:** `[35–60]`.  
   - **Livre:** `[60–64]` (apenas 4 KB, insuficiente para outros processos).

4. **P4 (10 KB)**  
   - Não encontra espaço adequado na RAM.  
   - **Destino:** memória virtual (disco rígido).

5. **P5 (18 KB)**  
   - Também não cabe na RAM disponível.  
   - **Destino:** memória virtual.

---

#### 🧠 Memória Após Alocação Inicial:

**📍 RAM (64 KB)**  
```
[0–20]   → P1  
[20–35]  → P2  
[35–60]  → P3  
[60–64]  → Espaço livre (4 KB)
```

**💾 Disco (Memória Virtual)**  
```
P4 (10 KB)  
P5 (18 KB)
```

---

### 2. Paginação na Memória Virtual

Como a RAM foi insuficiente, os processos restantes foram divididos em páginas e armazenados no disco.

#### 🔧 Parâmetros:
- **Tamanho total da RAM:** 64 KB
- **Tamanho da página:** 4 KB

| Processo | Tamanho | Nº de Páginas |
|----------|---------|----------------|
| P1       | 20 KB   | 5 páginas      |
| P2       | 15 KB   | 4 páginas      |
| P3       | 25 KB   | 7 páginas      |
| P4       | 10 KB   | 3 páginas      |
| P5       | 18 KB   | 5 páginas      |

#### 📑 Tabela de Páginas Inicial:

| Página | Processo | Local |
|--------|----------|-------|
| 0–4    | P1       | RAM   |
| 5–8    | P2       | RAM   |
| 9–15   | P3       | RAM   |
| 16–18  | P4       | Disco |
| 19–23  | P5       | Disco |

---

### 3. Desfragmentação da RAM

#### 💥 Antes da Desfragmentação:
```
[0–20]   → P1  
[20–35]  → P2  
[35–60]  → P3  
[60–64]  → Livre
```

- Fragmentos pequenos limitam novas alocações.
- Espaço livre total: 4 KB

#### 🧹 Desfragmentando:
A memória é reorganizada, movendo os processos para o início, eliminando espaços entre eles.

**Após compactação (RAM já estava organizada):**
```
[0–60]   → P1, P2, P3 (ocupando em sequência)
[60–64]  → Livre
```

Nada muda, pois os blocos já estavam contíguos.

---

#### 🧪 Simulação: Finalizando o Processo P2 (15 KB)

**Situação após liberar P2:**
```
[0–20]   → P1  
[20–35]  → Livre  
[35–60]  → P3  
[60–64]  → Livre
```

**Desfragmentando:**
```
[0–20]   → P1  
[20–45]  → P3  
[45–63]  → P5  
[63–64]  → Livre (1 KB)
```

Agora há **18 KB livres contíguos**, o que permite alocar o processo **P5**.

---

### 📊 Estado Atual da Memória (Pós-Desfragmentação)

**RAM:**
```
[0–20]   → P1  
[20–45]  → P3  
[45–63]  → P5  
[63–64]  → Livre
```

**Disco:**
```
P4 (10 KB)
```

#### 📘 Tabela de Páginas Atualizada:

| Página | Processo | Local |
|--------|----------|-------|
| 0–4    | P1       | RAM   |
| 5–11   | P3       | RAM   |
| 12–16  | P5       | RAM   |
| 17–19  | P4       | Disco |

### 4. Questões para Reflexão

**1. O algoritmo best-fit foi o mais eficiente nesse caso?**

No cenário apresentado, o best-fit teve um bom desempenho, mas sua vantagem real não ficou tão evidente. Como os processos foram alocados em sequência e havia blocos grandes disponíveis, praticamente qualquer algoritmo teria funcionado bem, inclusive o first-fit. O best-fit tenta sempre encaixar os processos nos menores espaços possíveis, o que ajuda a reduzir fragmentação, mas isso só faria diferença se já houvesse buracos pequenos sobrando. O worst-fit, por sua vez, tende a deixar fragmentos maiores e menos aproveitáveis, o que poderia ser um problema a longo prazo.

**2. De que forma a memória virtual evitou um possível deadlock?**

A memória virtual entrou como solução quando a RAM ficou sem espaço suficiente. Em vez de travar o sistema ou impedir novos processos, ela permitiu que o sistema armazenasse os processos restantes no disco. Isso evitou que os processos ficassem esperando indefinidamente por memória (ou seja, um deadlock). Graças a isso, o sistema conseguiu continuar operando normalmente, mesmo sem RAM suficiente para todos os processos.

**3. Qual o impacto da desfragmentação no desempenho?**

A desfragmentação ajuda a liberar espaço contíguo na RAM, o que permite alocar novos processos sem depender tanto da memória virtual. Isso melhora o desempenho, já que acessar a RAM é muito mais rápido do que acessar o disco. Por outro lado, desfragmentar também consome recursos, já que o sistema precisa mover dados de lugar. Se feita em um momento inadequado, pode até causar lentidão. Por isso, é uma técnica útil, mas deve ser usada com cautela, quando realmente houver fragmentação atrapalhando.


# fsync-conscious

> Conscious filesystem diff, audit & sync tool  
> Explicit > Blind  
> Auditável > Conveniente  
> Engenharia > Copiar e colar

---

## 📌 O que é

`fsync-conscious` é uma ferramenta de **diff, auditoria e sincronização consciente de diretórios**.

Ela existe porque **copiar e colar pastas é uma operação cega**.

Este projeto parte de um princípio simples:

> **Nunca sincronize o que você não entende.**

Antes de aplicar qualquer mudança, o fsync:
- calcula diferenças
- classifica riscos
- detecta erros reais de filesystem
- gera relatórios auditáveis
- falha de forma honesta quando necessário

---

## 🎯 Problema que resolve

Ferramentas comuns (`cp`, GUI, copy/paste):

- sobrescrevem silenciosamente
- não explicam o que mudou
- não detectam arquivos ilegíveis
- quebram em sockets, permissões e mounts
- não são auditáveis

`fsync-conscious` resolve isso tratando **filesystem como estado**, não como “conjunto de arquivos”.

---

## 🧠 Filosofia

- Diff explícito antes de sync
- Política clara > comportamento implícito
- Falhas são classificadas, não escondidas
- Dry-run deve produzir o MESMO relatório do sync real
- Hash é opcional, não dogma
- Nada é renomeado ou “corrigido” automaticamente

---

## 📦 Instalação

Clone o repositório:

```bash
git clone https://github.com/seu-user/fsync-conscious.git
cd fsync-conscious

#Instalação em modo desenvolvimento (CLI):
pip install -e .
```

## 🚀 Uso básico

Todos os comandos seguem o padrão:

```bash
fsync <mode> <A> <B> [options]
```

Onde:

A → diretório fonte (source of truth)

B → diretório destino (backup / espelho)

## 🔧 Modos de operação

1️⃣ diff — Exploração de diferenças

Mostra o que é diferente entre A e B.
Não cria, não copia, não remove nada.
```bash
fsync diff A B
```

Detecta:

- diretórios ausentes
- arquivos novos
- arquivos modificados
- arquivos extras no destino

2️⃣ sync — Sincronização consciente

Atualiza B com base em A.
```bash
fsync sync A B
```

Por padrão:
- não deleta nada
- só copia arquivos novos ou atualizados
- ignora entidades inválidas
- continua mesmo com erros pontuais

3️⃣ check — Validação de igualdade

Verifica se A e B são equivalentes.
```bash
fsync check A B
```

exit 0 → iguais

exit 1 → diferentes

Ideal para CI, sanity check e auditoria automatizada.

perfeito — segue **tudo isso convertido para Markdown limpo**, pronto pra colar direto no `README.md` ou em um doc separado (ex: `USAGE.md`).
Não mudei conteúdo, só **estruturei, normalizei títulos, listas e blocos de código**.

---

## 🔍 Estratégias de comparação

### 📅 Default: `mtime`

Por padrão, a comparação é feita usando:

- existência do arquivo  
- timestamp de modificação (`mtime`)

```bash
fsync sync A B
````

#### Vantagens

* rápido
* pouco I/O
* não exige permissão de leitura
* robusto para **99% dos casos reais**

---

### 🔐 `--hash` (SHA256)

Ativa comparação por conteúdo.

```bash
fsync diff A B --hash
```

Comportamento:

* lê o conteúdo completo dos arquivos
* detecta qualquer alteração real
* arquivos ilegíveis são marcados como `UNREADABLE`

⚠️ **Hash é opt-in por design.**

---

## 🧪 Modos de segurança

### `--dry-run`

Simula toda a operação, **sem escrever nada**.

```bash
fsync sync A B --dry-run
```

**Importante:**

* o relatório gerado é o **MESMO** do sync real
* apenas os efeitos colaterais são suprimidos

---

### `--strict-fs`

Qualquer erro vira **erro fatal**.

```bash
fsync sync A B --strict-fs
```

Abortará em:

* arquivo ilegível
* nome inválido para o filesystem destino
* erro de escrita
* incompatibilidade de mount

Ideal para **CI** e **backups críticos**.

---

## 🧾 Modo auditoria

### `--audit-only`

Gera relatório final + exit code explícito.

```bash
fsync sync A B --dry-run --audit-only
```

Exemplo de saída:

```
AUDIT REPORT
copied: 214
skipped: 0
unreadable: 2
invalid: 1
```

---

## 📊 Classificação de eventos

O fsync classifica tudo em quatro categorias:

| Categoria    | Significado                                             |
| ------------ | ------------------------------------------------------- |
| `copied`     | arquivos copiados (ou que seriam copiados em `dry-run`) |
| `skipped`    | ignorados por política                                  |
| `unreadable` | não foi possível ler                                    |
| `invalid`    | nome/path inválido para o filesystem destino            |

👉 **Nada é silencioso.**

---

## 🚦 Exit codes

Inspirados no `rsync`:

| Código | Significado                |
| ------ | -------------------------- |
| `0`    | sucesso total              |
| `1`    | diferenças encontradas     |
| `2`    | sync aplicado              |
| `10`   | arquivos pulados           |
| `20`   | arquivos ilegíveis         |
| `30`   | arquivos inválidos         |
| `99`   | erro fatal (`--strict-fs`) |

---

## 🧹 Espelhamento total

```bash
fsync sync A B --delete
```

Remove do destino tudo que não existe mais na fonte.

⚠️ **Use com cuidado.**

---

## 🔄 Inverter direção

```bash
fsync diff A B --reverse
```

Equivale a:

```
B → A
```

---

## 🚫 O que o fsync ignora por design

* sockets (`mysql.sock`)
* FIFOs
* devices
* symlinks quebrados
* arquivos removidos durante o scan

Filesystem é heterogêneo.
A ferramenta assume isso explicitamente.

---

## 🧠 Casos de uso reais

* backup incremental consciente
* validação de backups
* laptop ↔ HD externo
* staging ↔ produção
* auditoria de dados
* CI/CD
* ambientes Docker
* volumes montados
* dados críticos

---

## ❌ Quando NÃO usar

* cópia única e descartável
* tarefas triviais
* usuários leigos
* quando conveniência > controle

---

## 🧠 Insight final

> Copiar e colar resolve uma ação.
> **fsync resolve um processo.**

Essa ferramenta existe para quem precisa:

* entender o que acontece
* assumir responsabilidade
* auditar decisões
* evitar perda silenciosa de dados

---

## 📄 Licença

MIT — use, modifique e distribua.
Mas **entenda o que você está fazendo**.

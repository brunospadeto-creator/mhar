# Material para o depósito no Zenodo

Cópia de tudo o que vai para o Zenodo junto com o artigo. Só este arquivo entra no git; o resto está no
`.gitignore`, porque é volumoso ou repete arquivos que já estão versionados. Para refazer a pasta, veja o fim
deste arquivo.

## O que enviar

Envie os arquivos `.zip` e `.tar.gz` abaixo. O Zenodo não aceita pastas. As pastas desempacotadas que estão ao
lado deles servem só para conferência.

| Parte | Arquivo a enviar | Tamanho | Conteúdo |
|---|---|---|---|
| 1. Código e protocolo | `1-codigo-e-protocolo/deposito-g-m-t-k-<data>-<commit>.zip` | 0,8 MB | código do método e de tudo o que ele importa, adaptadores das bases, scripts de avaliação e de robustez, comparadores, protocolo registrado, emendas, código congelado no registro, cronologia dos commits, relatórios de resultado, protocolos e coleta do TeaStore, ambiente travado e script de reprodução |
| 2. Evidências derivadas | `2-evidencias.tar.gz` | 230 MB (1,2 GB aberto) | o que as tabelas do artigo leem: registros por instante e artefatos por caso (`jsonl.gz`), escores e caches (`.pkl`), saídas dos comparadores |
| 3. Coletas do TeaStore | `3-coletas-teastore/*.tar.gz` (10 arquivos) | 1,4 GB (cerca de 27 GB aberto) | telemetria bruta das 8 rodadas usadas no artigo, baselines de rastros normais das duas coleções e arquivos de controle |
| 4. Material suplementar | `4-material-suplementar.zip` | 0,4 MB | figuras do artigo (PDF, EPS e PNG, em Arial) com o script que as gera, e o glossário dos nomes (`NOMES.md`) |

O total, cerca de 1,6 GB, fica bem abaixo do limite de 50 GB por registro do Zenodo.

## Conteúdo de cada parte

### 1. Código e protocolo
É o pacote gerado por `../deposito/montar.sh` a partir de um commit. A lista do que entra está em
`../deposito/arquivos.txt`: o fecho dos imports dos scripts que produzem os números do artigo e do material
suplementar, os protocolos, os relatórios de resultado e a coleta do TeaStore. Os comentários passam pelo
`../deposito/limpar.py`, que retira as referências às notas internas e confere que a AST de cada arquivo Python não
mudou; o código congelado no registro não passa pela limpeza. O `LEIAME.md` de dentro do pacote explica a estrutura e
como reproduzir.

Testes do pacote (21/10/2026): o confirmatório RCAEval saiu idêntico, linha por linha, ao relatado, num contêiner
sem rede; as nove etapas do TeaStore, rodadas a partir do pacote e da parte 3, deram relatórios, calibrações e cache
da robustez idênticos, byte a byte, aos da parte 2.

### 2. Evidências derivadas (`2-evidencias/`)
| Pasta | Origem na máquina do autor | Conteúdo |
|---|---|---|
| `teastore/protocolo-coleta-definitiva` | `.../stage3-online/resultados/protocolo-coleta-definitiva` | os 24 cenários da coleta definitiva, a tabela única e a variante sem eventos do Kubernetes |
| `teastore/protocolo-metricas-rz` | `.../resultados/protocolo-metricas-rz` | calibração, inéditos (B′), coleta definitiva, extensão com eventos e cache da robustez |
| `teastore/protocolo-confirmatorio` | `.../resultados/protocolo-confirmatorio` | confirmatório anterior dos métodos de rastros (21 cenários); fora das tabelas do artigo |
| `rcaeval` | `/mnt/gpu-disk/datasets/bases-externas/rcaeval` | registros por instante e artefatos por caso do RCAEval |
| `openrca-market` | `/mnt/gpu-disk/datasets/bases-externas/openrca-market` | plano de casos, registros e artefatos do OpenRCA Market |
| `metricas-rz` | `/mnt/gpu-disk/datasets/bases-externas/metricas-rz` | escores por família, portões, robustez (`robustez/`) e saídas do CIRCA, do RCD, do MicroRank e do TraceRCA (`outros-comparadores/`) |
| `d9a`, `pre-teste`, `baseline-dinamica` | `/mnt/gpu-disk/datasets/bases-externas/...` | rodadas do desenvolvimento e do pré-teste citadas nos relatórios de resultado |
| `rodar-*.log` | idem | registros de execução das rodadas nas bases externas |

Os relatórios usam os códigos internos dos métodos (d9-k, v-cons-b…); o `NOMES.md` da parte 1 os traduz
para os nomes do artigo. Os `.pkl` são do pickle do Python e são caches: os scripts os regeneram a partir dos dados.
As subpastas `eadro` e o `rodar-eadro.log` são o registro do conjunto SN do Eadro, avaliado no desenvolvimento e
relatado no material suplementar (Seção S6).

### 3. Coletas do TeaStore (`3-coletas-teastore/`)
`SHA256SUMS` confere cada arquivo.

| arquivo | conteúdo |
|---|---|
| `teastore-20261013T051436Z`, `…061915Z`, `…143247Z`, `teastore-20261016T075241Z` | as rodadas dos 17 cenários inéditos (B′) |
| `teastore-20261016T002739Z`, `…151716Z`, `…223053Z`, `teastore-20261017T050131Z` | as rodadas dos 24 cenários da coleta definitiva |
| `teastore-baselines.tar.gz` | rastros normais das baselines das coleções `547ba505` (B′) e `32c273a6` (definitiva) |
| `teastore-controle.tar.gz` | marcadores dos lotes da coleta definitiva (`definitiva/C*.run`, `C*.done`) e artefatos da baseline dessas duas coleções (`insumos/`) |

Os arquivos são JSON. A varredura por senhas, tokens e chaves não achou segredos: a única senha que aparece é a
pública dos usuários de demonstração do TeaStore (`name=user<n>&password=password`, nos rastros do login e no SQL do
próprio benchmark), e `authorization_scope` é um campo do projeto. Os rastros têm endereços IP internos do cluster
de teste.

### 4. Material suplementar (`4-material-suplementar/`)
As figuras do artigo, geradas em 21/10/2026 com Arial embutida (PDF e EPS vetoriais, PNG para conferência), o
script que as gera e o glossário dos nomes.

## O que não vai

- **Dados públicos brutos** (RCAEval, OpenRCA Market): já têm DOI e licença próprios; o artigo os cita. Pelo mesmo
  motivo saíram as fatias `parquet` do OpenRCA Market (589 MB), que são recortes dos dados públicos.
- **Eadro bruto**: conjunto fora das tabelas do artigo.
- **Rodadas do TeaStore fora das tabelas** (desenvolvimento e confirmatório anterior dos métodos de rastros).
- **`main.pdf`**: vai para o periódico, não para o Zenodo.

## Antes de enviar (autor)

1. Reservar o DOI no Zenodo e escolher a licença (proposta: MIT para o código, CC BY 4.0 para dados e textos).
2. Trocar os dois `\pendente{DOI}` do `main.tex` (Data e Code availability) pelo DOI reservado.

## Como refazer esta pasta

1. `bash ../deposito/montar.sh [commit]` gera o pacote da parte 1 em `/mnt/gpu-disk/datasets/deposito-g-m-t-k/`.
2. A parte 2 copia as origens da tabela acima, sem `openrca-market/fatias` e sem `__pycache__`.
3. A parte 3 compacta, com `tar` e `pigz -9`, cada `run-id` da tabela a partir de
   `/mnt/gpu-disk/veritas/teastore/stage3-online/resultados/`, as pastas `baseline-90m-20261003T223714Z` e
   `baseline-90m-20261015T165318Z` de `/mnt/gpu-disk/backups-misete/`, e, para o controle,
   `definitiva/C[1-4].{run,done}` e `insumos/abordagens-{32c273a6,547ba505}-*`.

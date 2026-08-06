# PepoResolve

Ferramentas de campanha da MKTRENDZ, client-side, em duas abas:

1. **Encurtador de links** — limpa links de Instagram e TikTok, removendo os
   parâmetros de rastreio (tudo depois do `?` na URL).
2. **Briefing** — recebe o briefing bagunçado do e-mail e remonta no modelo padrão.

**Tudo roda no navegador do usuário. Nenhum dado é enviado para servidor algum.**

---

# Aba 1 — Encurtador de links

## Como usar

1. Abra a página.
2. Cole a lista de links no campo de texto. Aceita dois formatos:
   - **Tabela**: colunas separadas por tab (colado direto de uma planilha).
   - **Texto solto**: URLs separadas por espaço ou quebra de linha (colado de um chat).
     Nesse caso as linhas são reconstruídas agrupando por
     perfil do Instagram → post do Instagram → todos os TikToks até o próximo perfil.
3. Clique em **Processar**.
4. Copie o resultado em TSV, copie uma coluna específica, ou baixe como `.tsv`.

## Coluna de nome (opcional)

Se a lista trouxer o nome do perfil antes dos links — seja como primeira coluna
da tabela, seja como as primeiras palavras da linha no texto solto — ele é
reconhecido e vira sempre a **primeira coluna**, com o cabeçalho `Nome`.

As iniciais de nome e sobrenome recebem maiúscula (`maria da silva` →
`Maria da Silva`). Partículas portuguesas (`de`, `da`, `do`, `das`, `dos`, `e`…)
ficam minúsculas quando não são a primeira palavra, e nomes com hífen são
tratados parte a parte (`ana-maria` → `Ana-Maria`).

O resto de cada palavra é preservado como veio, para não estragar grafias
próprias como `McCartney` ou `iFood` — ou seja, um nome digitado todo em
CAIXA ALTA continua em caixa alta.

## Campanhas de volume

Em campanha de volume o mesmo micro manda várias postagens, e a planilha de
respostas do formulário tem até 24 colunas:

```
Carimbo | Nome completo | Perfil IG | Post IG 1…10 | Perfil TikTok | Post TikTok 1…10
```

Não existe aba separada para isso: os nomes de coluna **não são atribuídos por
posição fixa**, então micro, médio e volume passam pela mesma aba.

- **Colando com o cabeçalho da planilha**, ele é aproveitado como está — é a
  fonte mais confiável de nome de coluna que existe.
- **Sem cabeçalho**, cada coluna é classificada pelo conteúdo e numerada:
  `Instagram - Post 1`, `Instagram - Post 2`, … Uma coluna vazia (o micro
  postou menos que o máximo) herda o tipo da anterior, mantendo a numeração.

Colunas que não são link — o carimbo de data/hora, por exemplo — ficam
intactas: só a coluna de nome recebe as iniciais maiúsculas.

## Regras de limpeza

- Cada URL é cortada no primeiro `?`, preservando tudo antes dele (inclusive a
  barra final, quando existir).
- URLs sem `?` ficam intactas.
- Links em formato markdown `[texto](url)` têm a URL real extraída dos parênteses.

## Validação

Coluna cujo cabeçalho contém **Perfil** não deveria ter link de postagem. Se
tiver, a célula é destacada em vermelho e um aviso lista as linhas suspeitas —
sinal de que o link do post foi colado no lugar errado.

Quem decide é o próprio link, não o rótulo da coluna: um post é `/p/` ou
`/reel/` no Instagram, e `/video/` **ou um link curto `vt.tiktok.com` /
`vm.tiktok.com`** no TikTok. Assim a checagem funciona mesmo com cabeçalho
abreviado como `perfil tt`, que não traz o nome da rede.

---

# Aba 2 — Organizador de briefing

Cola o briefing como ele chegou (e-mail, chat) e devolve no modelo padrão,
pronto para enviar ao influenciador.

## O que ele resolve na entrada

- **Lixo de e-mail colado**: o caractere `￼` (imagem perdida) é descartado.
- **Links markdown invertidos**: no e-mail original o texto do link é a URL boa
  e o `href` é um amontoado de URLs grudadas. Quando o texto é uma URL, ele
  vence; quando é palavra (`[Sugestão](lixo) de Legendas`), o texto é mantido e
  o `href` descartado.
- **Legendas numeradas** viram lista com marcador, agrupadas pelos títulos
  `Legendas para ...`. Itens vazios (um `11.` solto, por exemplo) são ignorados.
- **Orçamento e total de micros** ficam de fora do briefing final — são
  informação interna. Aparecem à parte, só para conferência.

## Regras vindas do rascunho oficial

O modelo `.docx` de briefing define as regras abaixo, que a ferramenta aplica:

- **Período**: *"sempre colocar data de início e fim"*, e sai sempre no formato
  `dd/mm - dd/mm`.
- **Sugestões de conteúdo**: *"adicionar pelo menos 3"* — abaixo disso entra
  como recomendação, não como pendência: na prática há campanha que sai com 1.
- **Link do formulário**: não vem no briefing do cliente, é a equipe que
  acrescenta no fim. Nunca é cobrado como falta, só lembrado.
- **Menção, Links da Música e Minutagem** são preenchidos com o que veio no
  briefing; quando faltam, entram na lista de pendências.
- A **Hashtag** fica entre a Menção e os Links da Música. Nem toda campanha
  tem uma, então a ausência dela é só um lembrete, não uma pendência.

## Seções opcionais

Nem toda campanha usa todas as seções. **Distribuição semanal de micros,
vídeos de referência e sugestões de legendas** só aparecem no resultado quando
existem no briefing, e a ausência delas não é apontada como pendência — não
estão no rascunho oficial e há campanha que sai sem nenhuma das três.

## Rótulos reconhecidos

O mesmo campo aparece com nomes diferentes conforme quem escreve o briefing:

| Campo | Rótulos aceitos |
| --- | --- |
| Período | `Período da Campanha`, `Período`, `Data`, `Datas`, `Data da campanha` |
| Total de micros | linha solta `120 Micros`, ou `Campanha: 100 micros` |
| Custo | `Custo`, `Orçamento`, `Investimento` — informação interna |
| Sugestões de conteúdo | bloco após `Briefing`, rótulo `Sugestões de Conteúdo`, ou `Trend`, `Tema` |

## Como a menção é decidida

1. Um `@` colado num rótulo de menção, ou as sub-linhas `Instagram:` / `TikTok:`.
2. Um `@` solto no texto — **fora de URLs**, porque o `@` de
   `tiktok.com/@fulano/video/...` é o autor do vídeo de referência, não o
   perfil a ser marcado.
3. Não havendo `@` nenhum, uma instrução de não marcar (`nem marcar ele!`)
   vira `Não é para marcar o artista`.

`NÃO MARCAR NA LEGENDA` não conta como proibição: restringe onde marcar, não
se deve marcar.

## Campos que aceitam texto livre

- **Menção**: pode ser `@perfil` (sai dividido em Instagram e TikTok) ou uma
  instrução (`Não é para marcar o artista`), que sai numa linha só.
- **Minutagem**: aceita horário solto (`00:17`, que vira `a partir de 00:17`),
  intervalo (`00:24 - 00:53`) ou observação junto, preservados como vieram.
  Vindo um valor só, sem separar por rede, ele fica no Instagram e o TikTok
  recebe `a partir de 00:00`, que é o usual — com um lembrete para conferir.
  Não havendo minutagem nenhuma, nada é preenchido: os dois ficam pendentes.
- **Link do formulário**: reconhecido pelo rótulo ou por ser um link do Google
  Forms. Mantém a query, que faz parte do endereço.

## Itens pendentes

Nada é inventado. O que não estiver no texto é listado como pendente e pode ser
preenchido nos campos acima do resultado, que atualiza na hora. Tipicamente
faltam **período** (o e-mail costuma trazer só a duração, sem data de início) e
**minutagem**.

Digitando a data de início, a data final é calculada a partir do número de
semanas do próprio briefing (3 semanas → `23/07` vira `23/07 - 13/08`).

## Conferência automática

Se a soma dos micros das semanas não bater com o total informado no briefing,
isso é apontado.

---

## Identidade visual

Segue a marca MKTRENDZ, com as cores amostradas dos arquivos oficiais:

| Cor | Onde vem |
| --- | --- |
| `#010400` | fundo das capas de apresentação |
| `#00D098` | verde do selo e da estrela — o acento da marca |
| `#1EAB79` → `#0EA7A7` | gradiente do logotipo, usado no nome |
| `#00522B` | brilho verde que sobe do rodapé |

A tipografia é **Montserrat**, a oficial da marca, embutida no HTML como
data URI: a página não faz nenhuma requisição externa, então continua
funcionando offline e sem depender de CDN.

O fundo reproduz as capas — preto quase absoluto, grade fina e o brilho verde
subindo do rodapé — e a estrela de quatro pontas do material da marca aparece
ao lado do nome.

## Estrutura

Arquivo único, sem build e sem dependências externas:

- `index.html` — HTML, CSS, JS e fontes da ferramenta inteira.
- `.nojekyll` — evita que o GitHub Pages processe os arquivos com Jekyll.

Para rodar localmente, basta abrir `index.html` no navegador.

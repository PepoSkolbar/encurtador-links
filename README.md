# Organizador — Instagram / TikTok

Ferramentas de campanha client-side, em duas abas:

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

## Regras de limpeza

- Cada URL é cortada no primeiro `?`, preservando tudo antes dele (inclusive a
  barra final, quando existir).
- URLs sem `?` ficam intactas.
- Links em formato markdown `[texto](url)` têm a URL real extraída dos parênteses.

## Validação

Se um link em qualquer coluna **Perfil** contiver `/reel/` ou `/p/` (Instagram)
ou `/video/` (TikTok), a célula é destacada em vermelho e um aviso lista as
linhas suspeitas — sinal de que o link do post foi colado no lugar errado.

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

- **Período**: *"sempre colocar data de início e fim"*.
- **Sugestões de conteúdo**: *"adicionar pelo menos 3"* — abaixo disso entra
  como recomendação, não como pendência: na prática há campanha que sai com 1.
- **Período**: sai sempre como `dd/mm - dd/mm`.
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

## Campos que aceitam texto livre

- **Menção**: pode ser `@perfil` (sai dividido em Instagram e TikTok) ou uma
  instrução (`Não é para marcar o artista`), que sai numa linha só.
- **Minutagem**: aceita horário solto (`00:17`, que vira `a partir de 00:17`),
  intervalo (`00:24 - 00:53`) ou observação junto, preservados como vieram.
- **Link do formulário**: reconhecido pelo rótulo ou por ser um link do Google
  Forms. Mantém a query, que faz parte do endereço.

## Itens pendentes

Nada é inventado. O que não estiver no texto é listado como pendente e pode ser
preenchido nos campos acima do resultado, que atualiza na hora. Tipicamente
faltam **período** (o e-mail costuma trazer só a duração, sem data de início) e
**minutagem**.

Digitando a data de início, a data final é calculada a partir do número de
semanas do próprio briefing (3 semanas → `23/07` vira `23/07 a 13/08`).

## Conferência automática

Se a soma dos micros das semanas não bater com o total informado no briefing,
isso é apontado.

---

## Estrutura

Arquivo único, sem build e sem dependências externas:

- `index.html` — HTML, CSS e JS da ferramenta inteira.
- `.nojekyll` — evita que o GitHub Pages processe os arquivos com Jekyll.

Para rodar localmente, basta abrir `index.html` no navegador.

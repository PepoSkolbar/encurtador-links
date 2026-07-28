# Encurtador de links — Instagram / TikTok

Ferramenta client-side que limpa links de Instagram e TikTok, removendo os
parâmetros de rastreio (tudo depois do `?` na URL).

**Tudo roda no navegador do usuário. Nenhum dado é enviado para servidor algum.**

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

## Estrutura

Arquivo único, sem build e sem dependências externas:

- `index.html` — HTML, CSS e JS da ferramenta inteira.
- `.nojekyll` — evita que o GitHub Pages processe os arquivos com Jekyll.

Para rodar localmente, basta abrir `index.html` no navegador.

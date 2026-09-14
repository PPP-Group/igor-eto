# Igor Eto · 7077 · Landing page da campanha

Single-page em HTML/CSS/JS puro, sem framework e sem build. Para publicar, basta
subir a pasta inteira para qualquer hospedagem estática.

```
ze-reis-site/
├── index.html
├── style.css
├── script.js
└── assets/
    ├── foto-2.jpg                     ← retrato oficial, seção "Quem é"
    ├── evento-1..6 .jpg               ← galeria (retrato/paisagem, comprimidas)
    ├── hero-igor.png                  ← foto do candidato no hero (fundo transparente)
    └── favicon.svg
```

Rodando localmente:

```bash
node .claude/static-server.js
```

## Pendências para preencher

Todos os pontos abaixo estão marcados com `TODO` no código.

| Onde | O que falta |
|---|---|
| `script.js` · Formulário | `APPS_SCRIPT_URL` ainda é um placeholder. Ver seção "Formulário → Google Sheets" abaixo. |
| `index.html` · Trabalho Realizado | 4 realizações com valor "Dados a confirmar" + 1 card totalmente pendente ("Próxima entrega"). |
| `index.html` · Redes sociais | Facebook, YouTube e TikTok foram removidos a pedido da campanha; só o Instagram real ficou. |

Já preenchidos com dados reais: CNPJ da campanha, WhatsApp oficial (hero, Participe e botão
flutuante) e e-mail da campanha (card "E-mail" em Participe). CPF, data de nascimento e
endereço residencial do candidato foram recebidos mas **não** entraram no site — são dados
sensíveis sem exigência legal de publicação numa landing page de campanha.

## Detalhes de implementação

- **Fotos**: as imagens de `assets/` vieram da pasta `FOTOS/` fornecida pela
  campanha (recomprimidas/recortadas para peso de web: algumas chegaram em
  vários MB e giraram sozinhas pela ausência de orientação EXIF correta).
- **Paleta**: azul navy (`#1A3A8F`/`#0A1628`) + amarelo (`#F5C400`) + branco.
  Sem laranja, vermelho ou rosa em nenhum elemento de campanha (o verde do
  WhatsApp é a única exceção, por convenção da própria marca do WhatsApp).
- **Posts do Instagram ("Nas redes")**: 3 posts fixos, embed oficial do
  Instagram (`instagram.com/embed.js`, sem widget de terceiro nem custo).
  São 3 links fixos escolhidos pela campanha, não atualizam sozinhos — pra
  trocar, é só editar o `data-instgrm-permalink` de cada `<blockquote>` no
  `index.html` pelo link do novo post.
- **Formulário de contato**: valida os campos e envia (`fetch` com
  `mode: 'no-cors'`) para um Google Apps Script Web App, que grava uma linha
  numa planilha do Google Sheets. Não abre mais o WhatsApp automaticamente.

  ### Formulário → Google Sheets (configuração)
  1. Crie uma planilha no [Google Sheets](https://sheets.google.com).
  2. Extensões → Apps Script → apague o conteúdo padrão e cole o script de
     [`docs/apps-script-formulario.gs`](docs/apps-script-formulario.gs).
  3. Implantar → Nova implantação → tipo "App da Web" → executar como você
     mesmo → acesso "Qualquer pessoa" → Implantar (autorize quando pedir).
  4. Copie a URL gerada (termina em `/exec`) e cole em `script.js`, na
     constante `APPS_SCRIPT_URL` (dentro da função `formulario()`).
  5. Compartilhe a planilha (Drive → Compartilhar) com quem for acompanhar
     as respostas.

  O script não envia e-mail nenhum, só grava na planilha (permissão
  mínima necessária: acesso ao Google Sheets). O mesmo script serve para
  outros sites de campanha — basta repetir os passos numa planilha nova
  para cada um.
  Como o Apps Script não responde com cabeçalhos CORS, o `no-cors` deixa a
  resposta opaca: o site não consegue confirmar o status HTTP, só se a
  requisição saiu da rede sem erro (por isso a validação de sucesso é
  "a promessa não rejeitou", não "o servidor confirmou 200").
- **Peso inicial**: CSS + JS + hero. As fotos da galeria carregam sob
  demanda com `loading="lazy"`.
- **Acessibilidade**: navegação por teclado no lightbox (setas e `Esc`), foco
  visível, contraste conferido para WCAG AA (inclusive nos elementos amarelos,
  que usam texto navy em vez de branco) e respeito a
  `prefers-reduced-motion`. Com JS desativado o conteúdo continua visível.

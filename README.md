# Igor Eto · 7077 · Landing page da campanha

Single-page em HTML/CSS/JS puro, sem framework e sem build. Para publicar, basta
subir a pasta inteira para qualquer hospedagem estática.

```
ze-reis-site/
├── index.html
├── style.css
├── script.js
└── assets/
    ├── foto-candidato-sem-fundo.png   ← recorte do candidato (fundo transparente), usado na urna
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
| `index.html` · Vídeos | 3 iframes com `data-src="URL_DO_VIDEO"`: trocar pela URL de embed. O carregamento lazy já está pronto no `script.js`. |
| `index.html` · Apoios | 3 cards de depoimento com nome, cargo e texto de exemplo. |
| `index.html` · Trabalho Realizado | 4 realizações com valor "Dados a confirmar" + 1 card totalmente pendente ("Próxima entrega"). |
| `index.html` · Redes sociais | Facebook, YouTube e TikTok com `href="#"` (Instagram já está preenchido com `instagram.com/igor.eto`). |

Já preenchidos com dados reais: CNPJ da campanha, WhatsApp oficial (hero, Participe e botão
flutuante) e e-mail da campanha (card "E-mail" em Participe). CPF, data de nascimento e
endereço residencial do candidato foram recebidos mas **não** entraram no site — são dados
sensíveis sem exigência legal de publicação numa landing page de campanha.

## Detalhes de implementação

- **Fotos**: as imagens de `assets/` vieram da pasta `FOTOS/` fornecida pela
  campanha (recomprimidas/recortadas para peso de web: algumas chegaram em
  vários MB e giraram sozinhas pela ausência de orientação EXIF correta).
  `foto-candidato-sem-fundo.png` usa um recorte com fundo já transparente
  entregue pela campanha ("Olhar Horizonte.png"); nenhum fundo foi removido
  manualmente neste projeto.
- **Urna eletrônica**: 100% CSS/SVG, sem imagem de fundo. Digita `7077`
  sozinha ao entrar na viewport, no hover ou no toque; aceita digitação manual
  pelo teclado numérico (4 dígitos); `CONFIRMA` dispara confetes e `CORRIGE`
  reinicia. O som (beeps via `AudioContext`) só toca depois da primeira
  interação do usuário na página, para respeitar as políticas de autoplay.
- **Paleta**: azul navy (`#1A3A8F`/`#0A1628`) + amarelo (`#F5C400`) + branco.
  Sem laranja, vermelho ou rosa em nenhum elemento de campanha (o verde do
  WhatsApp é a única exceção, por convenção da própria marca do WhatsApp).
- **Formulário de contato**: sem backend. Ao enviar, valida os campos, mostra a
  confirmação e abre o WhatsApp da campanha numa nova aba, já com nome, e-mail,
  telefone e mensagem preenchidos no texto (`wa.me/5531999267007?text=...`).
- **Peso inicial**: CSS + JS + hero. As fotos da galeria carregam sob
  demanda com `loading="lazy"`.
- **Acessibilidade**: navegação por teclado no lightbox (setas e `Esc`), foco
  visível, contraste conferido para WCAG AA (inclusive nos elementos amarelos,
  que usam texto navy em vez de branco) e respeito a
  `prefers-reduced-motion`. Com JS desativado o conteúdo continua visível.

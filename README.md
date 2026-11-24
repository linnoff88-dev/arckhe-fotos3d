# fotos3d â€” fotos comuns de um cÃ´modo â†’ sala em 360Â°

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


**ðŸ‡§ðŸ‡· [PortuguÃªs](README.md) Â· ðŸ‡ºðŸ‡¸ [English](README.en.md) Â· ðŸ‡ªðŸ‡¸ [EspaÃ±ol](README.es.md)**

VersÃ£o 1.0.0. Pega 4â€“6 fotos normais de um mesmo ambiente (ou sÃ³ uma descriÃ§Ã£o em texto) e gera uma panorÃ¢mica **equiretangular 2:1 (4096Ã—2048)** fotorrealista, fiel ao imÃ³vel, que vocÃª gira num viewer 360 como num tour virtual. Sem cÃ¢mera 360, sem scan, sem software 3D.

> Ã‰ **reconstruÃ§Ã£o por IA**, nÃ£o mediÃ§Ã£o: as Ã¡reas que nenhuma foto mostra sÃ£o inferidas de forma conservadora. Serve para marketing imobiliÃ¡rio, preview, conteÃºdo de tour virtual e insumo para vÃ­deo IA. NÃ£o substitui um scan LiDAR.

## ðŸ“– Guia de uso

Guia completo (landing + passo a passo): **https://linnoff88-dev.github.io/

Baseado no tutorial "Turn Multiple Property Photos Into a 360Â° Interior Image With Codex" (AI Video Lab). A diferenÃ§a: aqui a peÃ§a que o tutorial esconde (qual modelo de imagem gera a 360) estÃ¡ resolvida e testada.

## Como funciona

```
fotos (ou assunto) â†’ visÃ£o (Gemini) entende o cÃ´modo â†’ prompt conservador
â†’ modelo de imagem com as fotos como referÃªncia, 2:1 nativo
â†’ checagens (ratio exato, â‰¥4096Ã—2048, score de costura) â†’ suavizaÃ§Ã£o da costura â†’ PNG + relatÃ³rio
```

Modelos testados na mesma sala (Robie House, 6 fotos reais):

| id | provedor | 2:1 nativo | resultado | custo |
|---|---|---|---|---|
| `flux-klein` (padrÃ£o) | Freepik/Magnific | sim (2048Ã—1024, reamostrado p/ 4096) | equiretangular real, muito fiel | 10 crÃ©ditos |
| `gpt-image-2` | Freepik/Magnific | sim, atÃ© 4K, 16 refs | **melhor qualidade**: projeÃ§Ã£o esfÃ©rica correta, costura boa (8), ~80 s | ~45â€“90 crÃ©ditos |
| `or-gemini-pro` | OpenRouter | nÃ£o (16:9 â†’ 2:1) | 360 plausÃ­vel, menos fiel (mudou tapete e janelas), costura ruim | ~US$0,13 |
| `or-gemini-flash` | OpenRouter | nÃ£o | tende a sair grande-angular, nÃ£o 360 | ~US$0,04 |

## Uso

```bash
cd ~/projetos/fotos3d
pip install -r requirements.txt

# por fotos
python3 -m engine.pipeline /caminho/fotos-da-sala --nome sala-302

# por assunto (a IA cria 5 fotos coerentes e depois a 360)
python3 -m engine.pipeline --assunto "sala moderna, sofÃ¡ cinza em L, janela grande, piso de madeira clara" --nome demo

# interface web (upload, progresso, viewer 360, galeria)
uvicorn web.app:app --port 8360   # abre http://127.0.0.1:8360
```

SaÃ­da em `~/projetos/output/fotos3d/<nome>/`: `<nome>-360.png`, `analise.json`, `prompt.txt`, `relatorio.json`, `fotos/`.

Viewer avulso: abra `viewer/index.html` no navegador e arraste o PNG.

## Skill (Claude Code)

`skill/property-360/SKILL.md` â€” instale com `ln -s ~/projetos/fotos3d/skill/property-360 ~/.claude/skills/property-360`. Depois: *"usa a skill property-360 nas fotos da pasta X"*.

## Chaves

Carregadas em runtime de `~/projetos/wifi/.env`: `MAGNIFIC_API_KEY` (Freepik), `GEMINI_API_KEY` (visÃ£o), `OPENROUTER_API_KEY` (opcional). Nada Ã© copiado.

## Testes

```bash
python3 -m pytest -q tests
```

## Amostra

`samples/sala/` â€” 6 fotos da sala da Robie House (Frank Lloyd Wright), Wikimedia Commons, foto de w_lemay, CC BY-SA 2.0.

## LicenÃ§a

MIT.

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>
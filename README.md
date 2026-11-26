# fotos3d ê¢â¬â fotos comuns de um cêÂ´modo ê¢â â sala em 360êÂ°

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


**ê°Å¸â¡Â§ê°Å¸â¡Â· [PortuguêÂªs](README.md) êÂ· ê°Å¸â¡Âºê°Å¸â¡Â¸ [English](README.en.md) êÂ· ê°Å¸â¡Âªê°Å¸â¡Â¸ [EspaêÂ±ol](README.es.md)**

VersêÂ£o 1.0.0. Pega 4ê¢â¬â6 fotos normais de um mesmo ambiente (ou sêÂ³ uma descriêÂ§êÂ£o em texto) e gera uma panorêÂ¢mica **equiretangular 2:1 (4096êâ2048)** fotorrealista, fiel ao imêÂ³vel, que vocêÂª gira num viewer 360 como num tour virtual. Sem cêÂ¢mera 360, sem scan, sem software 3D.

> êâ° **reconstruêÂ§êÂ£o por IA**, nêÂ£o mediêÂ§êÂ£o: as êÂ¡reas que nenhuma foto mostra sêÂ£o inferidas de forma conservadora. Serve para marketing imobiliêÂ¡rio, preview, conteêÂºdo de tour virtual e insumo para vêÂ­deo IA. NêÂ£o substitui um scan LiDAR.

## ê°Å¸ââ Guia de uso

Guia completo (landing + passo a passo): **https://linnoff88-dev.github.io/

Baseado no tutorial "Turn Multiple Property Photos Into a 360êÂ° Interior Image With Codex" (AI Video Lab). A diferenêÂ§a: aqui a peêÂ§a que o tutorial esconde (qual modelo de imagem gera a 360) estêÂ¡ resolvida e testada.

## Como funciona

```
fotos (ou assunto) ê¢â â visêÂ£o (Gemini) entende o cêÂ´modo ê¢â â prompt conservador
ê¢â â modelo de imagem com as fotos como referêÂªncia, 2:1 nativo
ê¢â â checagens (ratio exato, ê¢â°Â¥4096êâ2048, score de costura) ê¢â â suavizaêÂ§êÂ£o da costura ê¢â â PNG + relatêÂ³rio
```

Modelos testados na mesma sala (Robie House, 6 fotos reais):

| id | provedor | 2:1 nativo | resultado | custo |
|---|---|---|---|---|
| `flux-klein` (padrêÂ£o) | Freepik/Magnific | sim (2048êâ1024, reamostrado p/ 4096) | equiretangular real, muito fiel | 10 crêÂ©ditos |
| `gpt-image-2` | Freepik/Magnific | sim, atêÂ© 4K, 16 refs | **melhor qualidade**: projeêÂ§êÂ£o esfêÂ©rica correta, costura boa (8), ~80 s | ~45ê¢â¬â90 crêÂ©ditos |
| `or-gemini-pro` | OpenRouter | nêÂ£o (16:9 ê¢â â 2:1) | 360 plausêÂ­vel, menos fiel (mudou tapete e janelas), costura ruim | ~US$0,13 |
| `or-gemini-flash` | OpenRouter | nêÂ£o | tende a sair grande-angular, nêÂ£o 360 | ~US$0,04 |

## Uso

```bash
cd ~/projetos/fotos3d
pip install -r requirements.txt

# por fotos
python3 -m engine.pipeline /caminho/fotos-da-sala --nome sala-302

# por assunto (a IA cria 5 fotos coerentes e depois a 360)
python3 -m engine.pipeline --assunto "sala moderna, sofêÂ¡ cinza em L, janela grande, piso de madeira clara" --nome demo

# interface web (upload, progresso, viewer 360, galeria)
uvicorn web.app:app --port 8360   # abre http://127.0.0.1:8360
```

SaêÂ­da em `~/projetos/output/fotos3d/<nome>/`: `<nome>-360.png`, `analise.json`, `prompt.txt`, `relatorio.json`, `fotos/`.

Viewer avulso: abra `viewer/index.html` no navegador e arraste o PNG.

## Skill (Claude Code)

`skill/property-360/SKILL.md` ê¢â¬â instale com `ln -s ~/projetos/fotos3d/skill/property-360 ~/.claude/skills/property-360`. Depois: *"usa a skill property-360 nas fotos da pasta X"*.

## Chaves

Carregadas em runtime de `~/projetos/wifi/.env`: `MAGNIFIC_API_KEY` (Freepik), `GEMINI_API_KEY` (visêÂ£o), `OPENROUTER_API_KEY` (opcional). Nada êÂ© copiado.

## Testes

```bash
python3 -m pytest -q tests
```

## Amostra

`samples/sala/` ê¢â¬â 6 fotos da sala da Robie House (Frank Lloyd Wright), Wikimedia Commons, foto de w_lemay, CC BY-SA 2.0.

## LicenêÂ§a

MIT.

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>
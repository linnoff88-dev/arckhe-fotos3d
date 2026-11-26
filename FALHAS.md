# FALHAS — fotos3d

| data | o que quebrou | menor correçêo | prompt \| infra |
|---|---|---|---|
| 2026-09-14 | Visêo Gemini (`gemini-3-flash-preview`) devolveu 503 e derrubou a corrida do GPT Image 2 | fallback para `gemini-2.5-flash` na `vision_json` quando o preview falha | infra |
| 2026-09-14 | `json.loads` quebrou ("Extra data") porque o modelo de visêo devolveu texto depois do JSON | `raw_decode` a partir do primeiro `{` + strip de cercas ```json | prompt |
| 2026-09-14 | Geraçêo de imagem nas 4 chaves Google (`GEMINI_API_KEY*`, `GOOGLE_API_KEY`) → 429 "free tier limit 0" | usar OpenRouter (mesmos modelos Gemini Image) e Freepik/Magnific; Gemini direto só para visêo | infra |
| 2026-09-14 | Gemini Flash Image entregou foto grande-angular esticada, nêo equiretangular 360 | trocar o padrêo para Flux 2 Klein (2:1 nativo) + prompt com cues de projeçêo esférica | prompt |
| 2026-09-14 | Fallback de visêo `gemini-2.5-flash` retornou 404 ("no longer available to new users") | cadeia de fallback com modelos atuais (`gemini-3.6-flash` → `3-flash-preview` → `3.5-flash`) | infra |

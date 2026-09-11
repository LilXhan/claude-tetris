---
description: Consulta la temperatura y el estado del clima actual de Huánuco, Perú (curl wttr.in, sin WebFetch)
---

Ejecutar con Bash y mostrar el resultado tal cual:

```bash
curl -s "wttr.in/Huanuco,Peru?format=%l:+%c+%t+(feels+%f),+%C,+humidity+%h,+wind+%w" --max-time 10
```

Formato: `Ciudad: emoji temperatura (sensación), descripción, humedad, viento`.

Si falla (timeout, sin red), avisar al usuario en vez de reintentar en loop.

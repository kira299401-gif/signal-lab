# Signal Lab V1.4.1 — registro técnico (sin alterar las señales)

Esta modificación de V1.4 **NO cambia** la fórmula EMA/RSI/momentum, los filtros, los umbrales, la duración ni las reglas de WIN/LOSS.

## Qué se añade
- Al emitir `HIGHER`, `LOWER` o `ESPERAR`, guarda las ocho lecturas técnicas observadas en ese instante.
- Guarda además los ocho valores numéricos: EMA50, EMA200, RSI14, momentum en unidades de precio, resistencia, soporte, posición en rango (%) y volatilidad del rango (%).
- Registra puntuación (`score`), intensidad (`strength`), umbral requerido, y fecha del último precio (`marketDataAt`).
- Incluye estos datos en `/api/export` (columnas nuevas **después** de las ocho columnas anteriores).
- En `Today`, cada evaluación presenta un apartado desplegable con las ocho lecturas.

## Precaución: NO instalar a mitad de las 500 evaluaciones
**Primero** completar y exportar el historial de las 500 evaluaciones de V1.4. La actualización del servidor provoca un nuevo despliegue y un reinicio; sin un volumen persistente configurado (`DATA_DIR=/data` en un Railway Volume montado en `/data`) podría perderse el historial. El código nuevo no tiene capacidad para reconstruir las lecturas de señales antiguas.

Después de las 500 evaluaciones, realizar respaldo CSV y migración controlada. Recomendada una instancia separada para pruebas de V1.4.1.

## Instalación posterior
Sube `server.js`, `package.json` y `public/index.html` a las mismas rutas del repositorio, una vez terminado el experimento de 500 evaluaciones y tras respaldar CSV y `state.json` si corresponde. Railway deberá volver a desplegar.

**Seguridad:** la V1.4 original no tiene autenticación de controles y permite limpiar historial desde el dominio público. No conectar bróker ni claves a esta versión hasta implementar autenticación.

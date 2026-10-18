# Signal Lab V1.4 — servidor Railway (simulación)

## Instalación
1. Sube `server.js`, `package.json` y la carpeta `public` a la **raíz** de tu repositorio `signal-lab` en GitHub.
2. Railway → servicio `signal-lab` → Settings → Source: conecta ese repositorio/rama. El build detectará Node.js y ejecutará `npm start` (si no, configura Start Command = `npm start`).
3. Mantén el dominio público de Railway, con el puerto automático `PORT` (el servidor escucha en `0.0.0.0`).
4. Para conservar resultados después de reinicios/redeploys, crea un **Volume** en Railway montado en `/data` y agrega la variable `DATA_DIR=/data`. Si no, el archivo JSON puede perderse cuando Railway recree el contenedor.
5. Abre el dominio público. En Signal selecciona activo, duración, modo, intervalo y límite; pulsa `Iniciar automático`.

## Consideraciones
- El proceso hace consultas REST a Binance cada ~5 segundos. El estado `En vivo (servidor)` indica datos recientes, **no** transmisión WebSocket ni ejecución de órdenes.
- Una sola configuración y señal activa por servicio; los cambios se comparten entre todos los navegadores.
- Sin autenticación: cualquiera con acceso al dominio puede modificar la configuración y borrar historial. No publiques datos sensibles; agrega autenticación antes de uso compartido.
- Railway puede suspender o reiniciar el servicio según el plan, y el volumen requiere configuración explícita. Tras un reinicio, las señales vencidas sin cotización fresca se registran como `SIN DATOS`.
- Las evaluaciones cortas (5/15 segundos) se aproximan con muestreo de 5 segundos y no garantizan precio exacto al vencimiento. Para precisión se requiere almacenar ticks y liquidar al timestamp preciso.
- El sistema usa precios **spot** de Binance, no precios OTC ni de un bróker. Es una herramienta educativa, no una estrategia validada ni una promesa de rentabilidad.
- El conteo de efectividad excluye `EMPATE`, `SIN DATOS` y `ESPERAR` del denominador de WIN/LOSS.

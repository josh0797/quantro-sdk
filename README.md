# Quantro SDK

Distribución pública de **`@quantroos/sdk`** para construir proveedores MCP
(Model Context Protocol) con Node.js y conectarlos a Quantro.

Este repositorio contiene únicamente documentación y releases del SDK. No
contiene el código ni el historial del producto Quantro.

> Versión inicial: **0.1.0**, acceso anticipado. La API puede cambiar entre
> versiones menores. Esta versión se distribuye mediante GitHub Releases;
> no está publicada en el registro de npmjs.com.

## Instalación

Requiere Node.js 20 o superior y un proyecto con módulos ESM.

```bash
npm install https://github.com/josh0797/quantro-sdk/releases/download/v0.1.0/quantroos-sdk-0.1.0.tgz zod
```

La instalación conserva el nombre `@quantroos/sdk`, por lo que los imports no
cambian:

```js
import { createQuantroProvider, defineTool } from '@quantroos/sdk'
import { z } from 'zod'
```

Para comprobar la CLI local después de instalar:

```bash
npx --no-install quantro help
```

No uses `npm install @quantroos/sdk` ni `npx @quantroos/sdk` mientras la versión
no esté publicada en npmjs.com. El README incluido en el paquete y la ayuda de
la CLI conservan esos ejemplos del canal npm; para esta distribución usa la
URL anterior y `npx --no-install quantro`.

El archivo del SDK se descarga de GitHub. Sus dependencias, incluido el SDK
MCP y Zod, siguen descargándose desde el registro npm habitual.

## Qué incluye

- Definición de herramientas con schemas Zod.
- Proveedor MCP HTTP con verificación de firmas HMAC-SHA256.
- Límites de tamaño de solicitud y rechazo de timestamps fuera de ventana.
- Caché de ejecuciones repetidas dentro del proceso.
- Declaraciones de tipos TypeScript y CLI `quantro`.

Consulta la [guía de uso](docs/quickstart.md) y las
[notas de la versión 0.1.0](releases/v0.1.0.md).

La caché del SDK no sustituye un registro de idempotencia persistente: para
operaciones que escriben datos, deduplica por organización, conexión y clave
de idempotencia en tu propia base de datos.

## Verificar la descarga

La release incluye `SHA256SUMS.txt` junto al paquete. Descarga ambos y ejecuta:

```bash
shasum -a 256 -c SHA256SUMS.txt
```

SHA-256 del paquete `quantroos-sdk-0.1.0.tgz`:

```text
85215df1172b62fc421d464aca63f747e8f14df34d82cfe61a852b49e4229f8e
```

La suma permite comprobar la integridad de la descarga; no es una firma
digital independiente.

## Seguridad y soporte

No incluyas API keys ni contraseñas en el código, capturas o issues. Usa
variables de entorno y mantén habilitada la verificación de firmas cuando
expongas el proveedor a internet. No uses `insecureSkipSignature` en producción.

Para reportar una posible vulnerabilidad de forma privada, escribe a
[soporte@quantroos.com](mailto:soporte@quantroos.com). Para problemas de uso,
puedes abrir un issue sin datos sensibles.

## Licencia

MIT — ver [LICENSE](LICENSE).

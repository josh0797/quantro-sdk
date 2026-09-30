# Proveedor MCP con Quantro SDK 0.1.0

## 1. Instala el SDK

En un proyecto Node.js 20+ instala el paquete de la release y Zod:

```bash
npm install https://github.com/josh0797/quantro-sdk/releases/download/v0.1.0/quantroos-sdk-0.1.0.tgz zod
```

## 2. Define una herramienta de solo lectura

Guarda este ejemplo como `index.mjs`. La herramienta devuelve datos de ejemplo
en memoria; no consulta ni modifica cuentas reales.

```js
import { createQuantroProvider, defineTool } from '@quantroos/sdk'
import { z } from 'zod'

const providerId = process.env.QUANTRO_PROVIDER_ID
const apiKey = process.env.QUANTRO_API_KEY

if (!providerId || !apiKey) {
  throw new Error('Configura QUANTRO_PROVIDER_ID y QUANTRO_API_KEY')
}

const customers = new Map([
  ['demo', { id: 'demo', name: 'Cliente de ejemplo' }],
])

const getCustomer = defineTool({
  name: 'customer.get',
  description: 'Obtiene un cliente de ejemplo por ID',
  inputSchema: z.object({ id: z.string() }),
  readOnly: true,
  handler: async ({ id }) => customers.get(id) ?? { error: 'not_found' },
})

createQuantroProvider({
  providerId,
  apiKey,
  tools: [getCustomer],
}).listen(3000, '127.0.0.1')
```

## 3. Configura las credenciales y arranca el proveedor

Obtén las credenciales de tu proveedor externo en Quantro y configúralas como
variables de entorno mediante tu administrador de secretos. No las publiques
ni las incluyas en el historial Git. Después ejecuta:

```bash
node index.mjs
```

Si cargas un archivo `.env` con `node --env-file=.env index.mjs`, necesitas
Node.js 20.6 o superior. Mantén ese archivo fuera de Git.

## 4. Conecta el endpoint

El gateway de Quantro corre en la nube y no puede acceder al `localhost` de tu
equipo. Necesitas un endpoint HTTPS accesible para él. Mantén la validación de
firma activada al exponerlo.

La CLI incluye una utilidad de túnel que requiere `cloudflared` instalado en
tu equipo; no lo descarga por ti. Consulta su ayuda local con:

```bash
npx --no-install quantro help
```

El túnel hace público el puerto local mientras está activo. No lo abras con
`insecureSkipSignature` habilitado. No se abre ningún túnel automáticamente
durante la instalación del SDK.

La disponibilidad de la sección de proveedores externos depende del acceso
de tu cuenta y organización en Quantro. La verificación de esta release cubre
el paquete y sus pruebas locales, no una conexión de producción con tu cuenta.

## Operaciones de escritura

Para herramientas que modifican datos, valida las entradas y los permisos en
tu servidor. Persiste una clave única compuesta por organización, conexión y
`ctx.idempotencyKey`; la caché de ejecuciones del SDK es local al proceso y se
pierde tras un reinicio. No la interpretes como garantía de ejecución única
entre réplicas.

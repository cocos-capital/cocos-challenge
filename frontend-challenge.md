# cocos-challenge-frontend

**Resumen:**
Desarrollar una web/app que permita visualizar la informacion obtenida en los siguientes endpoints
- **/instruments**: La pantalla deberá mostrar el listado de instrumentos obtenidos por este endpoint. Mostrar ticker, nombre, ultimo precio y retorno (calculado usando el ultimo precio y el precio de cierre que devuelve el mismo endpoint)
- **/portfolio**: La pantalla deberá mostrar el listado de activos que devuelve el endpoint. Para cada uno de ellos mostrar el ticker, la cantidad de la posicion, el valor de mercado y la ganancia total (user el `avg_buy_price` como valor de compra)
- **/search**: Desarrollar un buscador de activos por ticker.
- **/orders**: Al hacer click en algun instrumento mostrar un modal formulario para enviar una orden (el metodo es un POST). La respuesta del POST va a tener un `status` que puede ser `PENDING`, `REJECTED`, `COMPLETED`. Mostrar el estado que devolvio.

**Agregar unit test en caso que sea necesario**

# Consideraciones funcionales
En relacion al diseño podes inspirarte en aplicaciones como coinbase.com, binance.com (lite), robinhood.com.


- Los precios de los activos tienen que estar en pesos.
- NO hace falta simular el mercado.
- Cuando un usuario envía una orden, es necesario enviar la cantidad de acciones que quiere comprar o vender. Permitir al usuario enviar la cantidad de acciones exactas o un monto total de inversión en pesos (en este caso, calcular la cantidad de acciones máximas que puede enviar, no se admiten fracciones de acciones).
- Las ordenes tienen un atributo llamado `side` que describe si la orden es de compra (`BUY`) o venta (`SELL`).
- Las ordenes tienen distintos estados (status): 
    - `NEW` - cuando una orden `LIMIT` es enviada al mercado, se envía con este estado.
    - `FILLED` - cuando una orden se ejecuta. Las ordenes market son ejecutadas inmediatamente al ser enviadas.
    - `REJECTED` - cuando la orden es rechazada por el mercado ya que no cumple con los requerimientos, como por ejemplo cuando se envía una orden por un monto mayor al disponible.
    - `CANCELLED` - cuando la orden es cancelada por el usuario.
- Cuando un usuario manda una orden de tipo MARKET, la orden se ejecuta inmediatamente y el estado es `FILLED`.
- Cuando un usuario manda una order de tipo LIMIT, la orden el estado de la orden tiene que ser `NEW`.
- Solo se pueden cancelar las ordenes con estado `NEW`.
- Si la orden enviada es por un monto mayor al disponible, la orden tiene que ser rechazada y guardarse en estado REJECTED. Tener en cuenta tanto el caso de compra validar que el usuario tiene los pesos suficientes y en el de venta validar que el usuario tiene las acciones suficientes.
- Las transferencias entrantes y salientes se pueden modelar como ordenes. Las transferencias entrantes tiene side `CASH_IN` mientras que las salientes side `CASH_OUT`.
- Cuando una orden es ejecutada, se tiene que actualizar el listado de posiciones del usuario.
- Para hacer el calculo de la tenencia y pesos disponibles utilizar todos los movimientos pertinentes que hay en la tabla `orders`, utilizando la columna `size`.
- El cash (ARS) esta modelado como un instrumento de tipo 'MONEDA'.
- En la tabla `marketdata` se encuentras los precios de los ultimos 2 dias de los instrumentos. El `close`, es el último precio de cada activo. Para calcular el retorno diario utilizar las columnas `close` y `previousClose`.
- Cuando se envia una orden de tipo `MARKET`, utilizar el último precio (`close`).
- Para calcular el valor de mercado, rendimiento y cantidad de acciones de cada posición usar las ordenes en estado `FILLED` de cada activo.

# Consideraciones técnicas
- Desarrollar la aplicación utilizando **Node.js** y **Typescript**
- Utilizar **React** con alguna herramienta de compilacion como vite.
- Utilizar **Redux** o **React Query**
- npm o yarn
- **sass** para los estilos
- La aplicacion tiene que ser responsive

# Datos de la api
- GET https://dummy-api-topaz.vercel.app/portfolio
- GET https://dummy-api-topaz.vercel.app/instruments
- GET https://dummy-api-topaz.vercel.app/search?query=DYC
- POST https://dummy-api-topaz.vercel.app/orders

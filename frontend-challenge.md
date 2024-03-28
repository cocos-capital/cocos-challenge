# cocos-challenge-frontend

**Resumen:**
Desarrollar una web/app que permita visualizar la informacion obtenida en los siguientes endpoints
- **/instruments**: La pantalla deberá mostrar el listado de instrumentos obtenidos por este endpoint. Mostrar ticker, nombre, ultimo precio y retorno (calculado usando el ultimo precio y el precio de cierre que devuelve el mismo endpoint)
- **/portfolio**: La pantalla deberá mostrar el listado de activos que devuelve el endpoint. Para cada uno de ellos mostrar el ticker, la cantidad de la posicion, el valor de mercado y la ganancia total (user el `avg_buy_price` como valor de compra)
- **/search**: Desarrollar un buscador de activos por ticker.
- **/orders**: Al hacer click en algun instrumento mostrar un modal con un formulario para enviar una orden (el metodo es un POST). Un usuario deberia poder definir si es una orden de compra o venta (`BUY` o `SELL`), el tipo de orden es `MARKET` o `LIMIT`, la cantidad de acciones a enviar y, solo si el tipo de orden es LIMIT, el precio a enviar. La respuesta del POST va a tener un `id` y un `status` que puede ser `PENDING`, `REJECTED` o `FILLED`. Mostrar el id y status que devolvió. (Abajo hay un ejemplo de los parametro a enviar en el body del post)

# Consideraciones funcionales
- Los precios de los activos estan en pesos.
- Cuando un usuario envía una orden, es necesario enviar la cantidad de acciones que quiere comprar o vender. Permitir al usuario enviar la cantidad de acciones exactas o un monto total de inversión en pesos (en este caso, calcular la cantidad de acciones máximas que puede enviar utilizando el ultimos precio, no se admiten fracciones de acciones).
- Las ordenes tienen distintos estados (status): 
    - `PENDING` - cuando una orden `LIMIT` es enviada al mercado, se envía con este estado.
    - `FILLED` - cuando una orden se ejecuta. Las ordenes market son ejecutadas inmediatamente al ser enviadas.
    - `REJECTED` - cuando la orden es rechazada por el mercado ya que no cumple con los requerimientos, como por ejemplo cuando se envía una orden por un monto mayor al disponible.
- Cuando un usuario manda una order de tipo `LIMIT`, el estado de la orden devuelto es `PENDING` o `REJECTED`.
- - Cuando un usuario manda una order de tipo `MARKET`, el estado de la orden devuelto es `REJECTED` o `FILLED`.
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
- **Agregar unit test en caso que sea necesario**
- En relacion al diseño podes inspirarte en aplicaciones como coinbase.com, binance.com (lite), robinhood.com.

# Datos de la api
- GET https://dummy-api-topaz.vercel.app/portfolio
- GET https://dummy-api-topaz.vercel.app/instruments
- GET https://dummy-api-topaz.vercel.app/search?query=DYC
- POST https://dummy-api-topaz.vercel.app/orders
  Ejemplo body 1
  `{
      instrument_id: 1,
      side: 'BUY', // 'SELL'
      type: 'MARKET',
      quantity: 1234
  }`
  Ejemplo body 2
  `{
      instrument_id: 1,
      side: 'SELL'
      type: 'LIMIT',
      quantity: 123,
      price: 84.5
  }`
    

# cocos-challenge-react_native

**Resumen:**
Desarrollar una App que permita visualizar la informacion obtenida en los siguientes endpoints
- **/instruments**: La pantalla deberá mostrar el listado de instrumentos obtenidos por este endpoint. Mostrar ticker, nombre, ultimo precio y retorno (calculado usando el ultimo precio y el precio de cierre que devuelve el mismo endpoint)
- **/portfolio**: La pantalla deberá mostrar el listado de activos que devuelve el endpoint. Para cada uno de ellos mostrar el ticker, la cantidad de la posicion, el valor de mercado, la ganancia y rendimiento total  (user el `avg_cost_price` como valor de compra)
- **/search**: Desarrollar un buscador de activos por ticker.
- **/orders**: Al hacer click en algun instrumento mostrar un modal con un formulario para enviar una orden (el metodo es un POST). Un usuario deberia poder definir si es una orden de compra o venta (`BUY` o `SELL`), el tipo de orden es `MARKET` o `LIMIT`, la cantidad de acciones a enviar y, solo si el tipo de orden es LIMIT, el precio a enviar. La respuesta del POST va a tener un `id` y un `status` que puede ser `PENDING`, `REJECTED` o `FILLED`. Mostrar el id y status que devolvió. (Abajo hay un ejemplo de los parametro a enviar en el body del post)

# Consideraciones funcionales
- En relacion al diseño podes inspirarte en aplicaciones como **coinbase.com, binance.com (lite), robinhood.com.**
- Los precios de los activos estan en pesos.
- Cuando un usuario envía una orden, es necesario enviar la cantidad de acciones que quiere comprar o vender. Permitir al usuario enviar la cantidad de acciones exactas o un monto total de inversión en pesos (en este caso, calcular la cantidad de acciones máximas que puede enviar utilizando el ultimos precio, no se admiten fracciones de acciones).
- Las ordenes tienen distintos estados (status): 
    - `**PENDING**` - cuando una orden `LIMIT` es enviada al mercado, se envía con este estado.
    - `**FILLED**` - cuando una orden se ejecuta. Las ordenes market son ejecutadas inmediatamente al ser enviadas.
    - `**REJECTED**` - cuando la orden es rechazada por el mercado ya que no cumple con los requerimientos, como por ejemplo cuando se envía una orden por un monto mayor al disponible.
- Cuando un usuario manda una order de tipo `LIMIT`, el estado de la orden devuelto es `PENDING` o `REJECTED`.
- Cuando un usuario manda una order de tipo `MARKET`, el estado de la orden devuelto es `REJECTED` o `FILLED`.
- Para calcular el valor de mercado de una posicion del portafolio usar `quantity * last_price`. Tener en cuenta que el parametro `avg_cost_price` es el precio de compra promedio, utilizarlo para calcular la ganancia (valor absoluto $) y rendimiento (%) total.

# Consideraciones técnicas
- Desarrollar la aplicación en **ReactNative**
- La aplicacion tiene que ser responsive
- **Agregar unit test en caso que sea necesario**

# Datos de la api
- GET https://dummy-api-topaz.vercel.app/portfolio
- GET https://dummy-api-topaz.vercel.app/instruments
- GET https://dummy-api-topaz.vercel.app/search?query=DYC
- POST https://dummy-api-topaz.vercel.app/orders
  ```
  Ejemplo body 1
  {
      instrument_id: 1,
      side: 'BUY',
      type: 'MARKET',
      quantity: 1234
  }
  Ejemplo body 2
  {
      instrument_id: 1,
      side: 'SELL'
      type: 'LIMIT',
      quantity: 123,
      price: 84.5
  }
  ```
    

# cocos-challenge-fullstack

**Resumen:**
Desarrollar una aplicación web que permita a los usuarios realizar un seguimiento de sus inversiones y enviar una orden al mercado. La aplicación consistirá en 3 pantallas:
- **Home**: en esta pantalla se mostrará el valor total de la cuenta de un usuario, sus pesos disponibles para operar y el listado de activos que posee, en cada uno se mostrará la cantidad de acciones, el valor total monetario de la posición ($) y el rendimiento total (%).
- **Search**: en esta pantalla el usuario podrá buscar cualquier activo por ticker. Se accederá a esta pantalla desde un botón en el Home.
- **Orden**: a esta pantalla se accede haciendo click en algún activo listado en el search o desde el listado de posiciones de la home. El usuario podrá enviar una orden de compra o venta del activo. La pantalla tendrá que soportar dos tipos de ordenes: market y limite. Las ordenes market no requieren que se envíe el precio ya que se ejecutara la orden con las ofertas del mercado, por el contrario, las ordenes limite requieren el envío del precio al cual el usuario quiere ejecutar la orden.

# Consideraciones funcionales
- Los precios de los activos tienen que estar en pesos
- NO hace falta simular el mercado
- Cuando un usuario envía una orden, es necesario enviar la cantidad de acciones que quiere comprar o vender. Permitir al usuario enviar la cantidad de acciones exactas o un monto total de inversión en pesos (en este caso, calcular la cantidad de acciones máximas que puede enviar, no se admiten fracciones de acciones)
- Las ordenes tienen un atributo llamado `side` que describe si la orden es de compra (`BUY`) o venta (`SELL`)
- Las ordenes tienen distintos estados (status): 
    - `NEW` - cuando una orden limite es enviada al mercado, se envía con este estado.
    - `FILLED` - cuando una orden se ejecuta. Las ordenes market son ejecutadas inmediatamente al ser enviadas y pasan a este estado.
    - `REJECTED` - cuando la orden es rechazada por el mercado ya que no cumple con los requerimientos, como por ejemplo cuando se envía una orden por un monto mayor al disponible
    - `CANCELLED` - cuando la orden es cancelada por el usuario
- Cuando un usuario manda una orden de tipo market, la orden se ejecuta inmediatamente y el estado es `FILLED`
- Cuando un usuario manda una order de tipo limit, la orden el estado de la orden tiene que ser `NEW`
- Solo se pueden cancelar las ordenes con estado `NEW`
- Si la orden enviada es por un monto mayor al disponible, la orden tiene que ser rechazada y guardarse en estado REJECTED. Tener en cuenta tanto el caso de compra (validar que el usuario tiene los pesos suficientes) como el de venta (validar que el usuario tiene las acciones suficientes)
- Las transferencias entrantes y salientes se pueden modelar como ordenes. Las transferencias entrantes tiene side = 'CASH_IN' mientras que las salientes side = 'CASH_OUT'
- Cuando una orden es ejecutada, se tiene que actualizar el listado de posiciones del usuario.
- Para hacer el calculo de la tenencia y pesos disponibles para operar utilizar todos los movimientos pertinentes que hay en la tabla `orders`
- El cash (ARS) esta modelado como un instrumento de tipo 'MONEDA'
- En la tabla marketdata se encuentras los precios de los ultimos 2 dias de los instrumentos. El `last`, es el último precio de cada activo. Para calcular el retorno diario utilizar las columnas `last` y `previousClose`.
- Cuando se envia una orden de tipo `MARKET`, enviar el último precio (last)
- Para calcular el valor de mercado, rendimiento y cantidad de acciones de cada posición usar las ordenes en estado `FILLED` de cada activo.

# Consideraciones técnicas
- Para el frontend utilizar React o Next.js
- Para la API REST utilizar Node.js y algún framework que facilite el desarrollo de la API
- Implementar un test funcional sobre la función para enviar una orden
- NO es necesario implementar autenticación de usuarios.
- Documentá cualquier suposición o decisión de diseño que consideres relevante.

# Base de datos
Ya hemos creado una base de datos con las siguientes tablas y algunos datos (pueden usar el archivo `database.sql` para crear y popular las tablas):
- **users**: id, email, accountNumber
- **instruments**: id, ticker, name, type
- **orders**: id, instrumentId, userId, side, size, price, type, status, datetime
- **marketdata**: id, instrumentId, high, low, open, last, previousClose, datetime


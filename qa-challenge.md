# cocos-qa-backend

**Resumen:**
Desarrollar suite de casos de prueba para validar el release de Dolar MEP 24/7.

# Consideraciones funcionales
La operatoria de dolar mep se realiza mediante la compra y venta de bonos, por lo tanto la ejecución se realiza durante el horario de mercado (lunes a viernes de 11 a 17).

Se ha desarrollado una funcionalidad nueva para que los usuarios puedan cargar ordenes de compra y venta de Dolar MEP en cualquier momento de la semana es decir 24x7. Las ordenes que se cargan por fuera del horario de mercado serán ejecutadas a un precio fijo ni bien abra el mercado.

Cualquier orden debe restar plata disponible al momento de crearse la orden, es decir si se crea una orden de compra de Dolar MEP con ARS, se tiene que restar los pesos disponibles que se van a usar para comprar los dolares. En cambio si se realiza una venta de dolar, se debe restar los dolares de esas venta.
Los pesos o dolares serán acreditados 1 día despues de **ejecutarse la orden**.

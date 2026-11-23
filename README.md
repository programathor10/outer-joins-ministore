# outer-joins-ministore

### ¿Por qué usaste `LEFT JOIN` para la Consulta 1 y no `INNER JOIN`? ¿Qué se perdería si usaras `INNER JOIN`?
Usé un `LEFT JOIN` porque necesitaba traer todos los registros de la primera tabla (`productos`) junto con sus ventas asociadas, incluso cuando no tuvieran coincidencias en `ventas`. Si utilizaba un `INNER JOIN`, se perderían los productos del catálogo que nunca se vendieron (`Hub USB-C 7p` y `Parlante Bluetooth`).

---

### ¿Por qué usaste `RIGHT JOIN` para la Consulta 2? ¿Qué tabla está a la izquierda y cuál a la derecha en tu consulta?
Usé `RIGHT JOIN` porque ahora necesitaba priorizar la tabla de la derecha (`ventas`) completa para detectar posibles registros huérfanos que dieran `NULL` en la tabla de la izquierda (`productos`). En mi consulta, `productos` está a la izquierda (después del `FROM`) y `ventas` está a la derecha (después del `JOIN`).

---

### ¿Qué representan los valores `NULL` en cada resultado? Explicá con un ejemplo concreto de los datos qué significa que `venta_id` sea `NULL` en la Consulta 1 y que `producto_id` de `productos` sea `NULL` en la Consulta 2.
Los valores `NULL` representan la ausencia de coincidencias en la tabla opuesta:
* **En la Consulta 1 (`venta_id` es `NULL`):** Significa que el artículo existe en el catálogo, pero nunca registró una venta. Por ejemplo, los productos `"Hub USB-C 7p"` y `"Parlante Bluetooth"` aparecen con `NULL` en los datos de venta.
* **En la Consulta 2 (`producto_id` de `productos` es `NULL`):** Significa exactamente lo opuesto: existe una operación registrada en `ventas` (`venta_id = 10`), pero el `producto_id` asociado no figura en la tabla `productos` (un registro huérfano por error de carga o porque el artículo fue eliminado del catálogo).

---

### ¿Cuándo usarías `FULL OUTER JOIN` en un caso real de negocio?
En una auditoría de inventario donde se cruza el **stock teórico registrado en el sistema** contra el **conteo físico del depósito**. El `FULL OUTER JOIN` permite detectar ambos tipos de anomalías en una sola consulta: los artículos que figuran en el sistema pero no aparecieron en el depósito (`NULL` en conteo físico, posible faltante) y los bultos encontrados físicamente que no estaban cargados en el sistema (`NULL` en sistema, sobrante sin registrar).

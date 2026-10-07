# Diseño Conceptual - Modelo Entidad/Relación: Tajinaste S.A.

A continuación, se detalla el diseño conceptual elaborado para la gestión de la red de viveros, detallando sus entidades, atributos y relaciones.

## Descripción de Entidades y Atributos

### Entidad: VIVERO
Esta entidad fuerte representa las instalaciones físicas y locales de la empresa Tajinaste S.A..
*   **Código Vivero:** Atributo identificador, el cual permite distinguir de forma unívoca cada recinto. *Dominio: Cadena alfanumérica (ej. `VIV-001`).*
*   **Nombre Vivero:** Atributo descriptor que indica cómo se llama la instalación. *Dominio: Cadena de texto (ej. `Vivero Norte`).*
*   **Georreferenciación:** Atributo compuesto para cumplir con los requisitos de localización, el cual se desglosa a su vez en dos atributos simples:
    *   **Latitud:** Atributo simple numérico (ej. `28.4853`).
    *   **Longitud:** Atributo simple numérico (ej. `-16.3159`).

### Entidad: ZONA
Representa las distintas áreas funcionales en las que se divide cada vivero (almacén, exterior, etc.). Al no tener sentido su existencia sin un vivero que la albergue, se modela como una entidad débil por dependencia en identificación.
*   **Código Zona:** Atributo discriminante de la zona dentro del vivero. *Dominio: Cadena alfanumérica corta (ej. `Z-EXT`).*
*   **Nombre Zona:** Atributo descriptor para nombrar el área. *Dominio: Cadena de texto (ej. `Zona Exterior`).*
*   **Georreferenciación:** Atributo compuesto que ubica específicamente esa zona.
    *   **Latitud:** Atributo simple numérico.
    *   **Longitud:** Atributo simple numérico.

### Entidad: PRODUCTO
Modela los artículos físicos disponibles para la venta (plantas, elementos de jardinería o artículos de decoración). Es una entidad fuerte.
*   **Código Producto:** Atributo identificador unívoco en el sistema. *Dominio: Cadena alfanumérica (ej. `PROD-9032`).*
*   **Precio:** Atributo descriptor que indica el valor de venta. *Dominio: Numérico decimal.*
*   **Categoría:** Atributo descriptor que clasifica el artículo. *Dominio: Cadena de texto.*

### Entidad: EMPLEADO
Esta entidad fuerte registra la información de los trabajadores que prestan servicio en la red de viveros.
*   **DNI:** Atributo identificador del trabajador. *Dominio: Cadena de 9 caracteres.*
*   **Nombre:** Atributo descriptor general. *Dominio: Cadena de texto.*

### Entidad: TAREA
Define las actividades concretas que el personal desempeña durante su jornada. Es una entidad fuerte.
*   **Código Tarea:** Atributo identificador. *Dominio: Cadena alfanumérica.*
*   **Nombre:** Atributo descriptor de la actividad. *Dominio: Cadena de texto.*
*   **Descripción:** Atributo descriptor que detalla la función. *Dominio: Cadena de texto larga.*

### Entidad: CLIENTE
Almacena el registro de los compradores de la empresa. Es una entidad fuerte.
*   **Código Cliente:** Atributo identificador unívoco. *Dominio: Cadena alfanumérica.*
*   **Fecha Registro:** Atributo descriptor del alta general. *Dominio: Fecha.*
*   **Volumen Compras:** Atributo descriptor de actividad comercial. *Dominio: Numérico decimal.*

### Entidad: MIEMBRO (Subentidad)
Modela de forma exclusiva a los clientes suscritos al programa Tajinaste Plus. Se relaciona mediante una jerarquía parcial con la entidad Padre *Cliente*.
*   **Fecha Alta:** Atributo descriptor del momento en que ingresan al programa. *Dominio: Fecha.*
*   **Fecha Baja:** Atributo descriptor para el control de salidas. *Dominio: Fecha.*
*   **Bonificaciones:** Atributo descriptor asignado en función de sus compras mensuales. *Dominio: Numérico decimal.*

### Entidad: PEDIDO
Representa las compras formalizadas que realizan los clientes y que deben ser gestionadas por los empleados. Es una entidad fuerte.
*   **Código Pedido:** Atributo identificador unívoco del pedido en el sistema. *Dominio: Cadena alfanumérica.*
*   **Precio Total:** Atributo descriptor que almacena el importe total de la compra. *Dominio: Numérico decimal.* *(Nota: en tu diagrama tienes el óvalo duplicado; recuerda borrar uno antes de entregar).*

---

## Descripción de las Relaciones

*   **Tiene (VIVERO - ZONA):** Relación de dependencia en identificación con cardinalidad global **1:N**. Un Vivero puede contener múltiples zonas `(1,N)`. Por su parte, la Zona tiene una participación de `(1,1)`, indicando que pertenece a un único Vivero de manera obligatoria.
*   **Dispone de (ZONA - PRODUCTO):** Relación de cardinalidad **N:M**. Una Zona puede disponer de múltiples productos `(1,N)` y un Producto puede estar ubicado en múltiples zonas `(1,N)`. Incluye el atributo propio **Cantidad Stock** para indicar el volumen exacto de un producto en un área determinada.
*   **Desempeña (ZONA - EMPLEADO - TAREA):** Relación ternaria que funciona como el registro de asignaciones del empleado. Un Empleado desempeña múltiples tareas a lo largo del tiempo `(0,N)` y una Tarea es desempeñada por múltiples empleados `(0,N)`. La Zona figura con una participación de `(1,1)` ya que un empleado realiza una tarea únicamente en una zona. De esta relación dependen los atributos propios **Puesto**, **Productividad**, **Fecha Inicio** y **Fecha Fin**, los cuales documentan el histórico de rendimiento.
*   **Gestiona (EMPLEADO - PEDIDO):** Relación de cardinalidad **1:N**. Un Empleado puede gestionar desde ninguno hasta múltiples pedidos `(0,N)`. Sin embargo, cada Pedido está vinculado a un único Empleado responsable `(1,1)`.
*   **Compra (CLIENTE - PEDIDO):** Relación de cardinalidad **1:N**. Un Cliente puede realizar múltiples pedidos en el sistema `(0,N)`, mientras que un Pedido siempre pertenece a un único Cliente `(1,1)`.
*   **Contiene (PEDIDO - PRODUCTO):** Relación de cardinalidad **N:M**. Un Pedido incluye al menos un producto y puede contener varios `(1,N)`, y un Producto puede haber sido vendido en ninguno o en múltiples pedidos `(0,N)`. Lleva asociados los atributos propios **Cantidad** y **Precio Venta** para detallar cada línea de compra.
*   **Jerarquía "Es un tipo de" (CLIENTE - MIEMBRO):** Relación de especialización con jerarquía parcial. Un Miembro es obligatoriamente un Cliente `(1,1)` en la rama superior, pero un Cliente puede o no ser Miembro del programa de fidelización `(0,1)` en la rama inferior.

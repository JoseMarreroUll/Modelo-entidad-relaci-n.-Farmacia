# Modelo Entidad-Relación: Gestión de Farmacia

## Descripción General
Este modelo entidad-relación representa el sistema de gestión de una farmacia. El objetivo es mantener un control detallado de los medicamentos disponibles, los clientes, los pedidos, y las relaciones entre estos elementos, como los laboratorios y las familias de medicamentos.

### Entidades Definidas

1. **Medicamento**
   - **Descripción**: Esta entidad representa cada medicamento que la farmacia tiene en su inventario. Incluye información sobre el tipo de medicamento, su disponibilidad y si requiere receta médica.
   - **Atributos**:
     - `Código (identificador)`: Identificador único del medicamento (ejemplo: M001, M002).
     - `Nombre (discriminante)`: Nombre comercial del medicamento (ejemplo: Paracetamol).
     - `Tipo de venta`: Indicador de si el medicamento requiere receta médica (ejemplo: "Sí", "No").
     - `Unidades en stock`: Número de unidades disponibles en inventario (ejemplo: 50).
     - `Unidades vendidas`: Número de unidades ya vendidas (ejemplo: 30).
     - `Tipo`: Forma farmacéutica del medicamento (ejemplo: comprimido, jarabe).
     - `Precio`: Precio unitario del medicamento (ejemplo: 5.99 EUR).

2. **Laboratorio**
   - **Descripción**: Los laboratorios son los proveedores de los medicamentos. La farmacia también puede fabricar sus propios medicamentos, en cuyo caso el laboratorio es la farmacia misma.
   - **Atributos**:
     - `Código (identificador)`: Identificador único del laboratorio (ejemplo: L001, L002).
     - `Nombre (discriminante)`: Nombre del laboratorio (ejemplo: LabFarma).
     - `Nombre de persona de contacto (compuesto)`: Nombre de la persona de referencia en el laboratorio (ejemplo: Juan Pérez).
     - `Teléfono (multivaluado)`: Número de contacto del laboratorio (ejemplo: +34 123 456 789).
     - `Dirección`: Información postal del laboratorio, con subatributos como:
       - `Calle`: Nombre de la calle.
       - `Número`: Número de edificio (ejemplo: Calle 8, Número 12).
     - `Postal`: Código postal (ejemplo: 28080).
     - `Fax`: Número de fax del laboratorio.

3. **Familia**
   - **Descripción**: Los medicamentos se agrupan en familias según el tipo de enfermedades que tratan.
   - **Atributos**:
     - `Nombre (identificador)`: Nombre de la familia (ejemplo: Antiinflamatorios).

4. **Pedido**
   - **Descripción**: Representa un pedido realizado por un cliente, que incluye los medicamentos comprados, la fecha de compra y el precio total.
   - **Atributos**:
     - `Código (identificador)`: Identificador único del pedido (ejemplo: P001).
     - `Fecha de compra`: Fecha en que se realizó el pedido (ejemplo: 2023-09-15).
     - `Precio total (calculado)`: Precio total del pedido.

5. **Cliente con crédito**
   - **Descripción**: Son los clientes que tienen una línea de crédito con la farmacia y realizan pagos a final de mes.
   - **Atributos**:
     - `Código (identificador)`: Identificador único del cliente (ejemplo: C001).
     - `Nombre`: Nombre del cliente (ejemplo: María García).
     - `Apellidos`: Apellidos del cliente (ejemplo: García López).
     - `Teléfono (multivaluado)`: Número de contacto del cliente.
     - `Fecha de pago`: Fecha en que el cliente debe hacer el pago (ejemplo: 2023-10-01).
     - `Datos bancarios`: Información de la cuenta bancaria del cliente.
     - `Dirección`: Dirección del cliente con subatributos `Calle`, `Número`, etc.

### Relaciones Definidas

1. **Relación: Medicamento-Laboratorio**
   - **Descripción**: Un medicamento pudo haber sido fabricado por un laboratorio.
   - **Cardinalidad (1, N)**: Un medicamento puede o no haber sido comprado de un laboratorio. Un laboratorio puede vender como mínimo un medicamento y como máximo "n" medicamentos.

2. **Relación: Medicamento - Pedido**
   - **Descripción**: Cada pedido contiene 1 o varios medicamentos.
   - **Cardinalidad (1, N)**: Un pedido puede incluir uno o múltiples medicamentos, pero un mismo medicamento puede pertenecer sólo a un pedido.

3. **Relación: Pedido - Cliente con crédito**
   - **Descripción**: Un pedido puede ser realizado por un cliente con crédito.
   - **Cardinalidad (1, N)**: Un pedido puede ser realizado por un único cliente y un mismo cliente puede realizar varios pedidos.

4. **Relación: Medicamento - Familia**
   - **Descripción**: Los medicamentos pertenecen a una familia, agrupándolos según el tipo de enfermedad que tratan.
   - **Cardinalidad (1, N)**: Un medicamento pertenece a una única familia y una familia puede tener uno o varios medicamentos.
  
5. **Relación: Farmacia Fabrica**
   - **Descripción**: Un medicamento pudo haber sido fabricado por la farmacia.
   - **Cardinalidad (1, N)**: Un medicamento puede o no haber sido fabricado por la farmacia. Una farmacia puede fabricar como mínimo un medicamento y como máximo "n" medicamentos.

### Ejemplos Ilustrativos del Dominio

- **Medicamento**:
  - Código: M001
  - Nombre: Ibuprofeno
  - Tipo de venta: No requiere receta
  - Unidades en stock: 120
  - Unidades vendidas: 45
  - Tipo: Comprimido
  - Precio: 7.50 EUR

- **Laboratorio**:
  - Código: L001
  - Nombre: FarmaLab
  - Persona de contacto: Marta Gómez
  - Teléfono: +34 987 654 321
  - Dirección: Calle 12, Número 20
  - Fax: +34 987 654 322
  - Postal: 28001

### Restricciones Semánticas Propuestas
- **Restricción de unicidad**: El `Código` de cada entidad (Medicamento, Laboratorio, Cliente, Pedido) debe ser único para evitar duplicación de información.
- **Restricción de relación de cliente con crédito**: Un cliente con crédito no puede realizar un pedido si tiene pagos atrasados.
- **Restricción de stock**: No se puede realizar un pedido si las `Unidades en stock` del medicamento son menores a las solicitadas en el pedido.
- **Restricción de receta**: Los medicamentos que requieran receta, no pueden ser vendidos sin ella.

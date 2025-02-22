# API de Mercado Público - Documentación Detallada

Esta documentación describe el funcionamiento de la API pública de Mercado Público (`api.mercadopublico.cl`), una interfaz que permite consumir información sobre **licitaciones** y **órdenes de compra** generadas en la plataforma de compras públicas de ChileCompra (`www.mercadopublico.cl`). Esta plataforma es utilizada por más de **850 organismos públicos** del Estado chileno para adquirir bienes y servicios de más de **118.000 proveedores**, de los cuales el 91% son micro y pequeñas empresas. La API es ideal para análisis de datos, desarrollo de aplicaciones o seguimiento de procesos de compras públicas.

## Tabla de Contenidos
1. [Características Generales](#características-generales)
2. [Autenticación](#autenticación)
3. [Formatos de Respuesta](#formatos-de-respuesta)
4. [Consultas de Licitaciones](#consultas-de-licitaciones)
   - [Tipos de Consultas](#tipos-de-consultas-licitaciones)
   - [Estados de Licitaciones](#estados-de-licitaciones)
   - [Etiquetas de Licitaciones](#etiquetas-de-licitaciones)
5. [Consultas de Órdenes de Compra](#consultas-de-órdenes-de-compra)
   - [Tipos de Consultas](#tipos-de-consultas-ordenes)
   - [Estados de Órdenes de Compra](#estados-de-órdenes-de-compra)
   - [Etiquetas de Órdenes de Compra](#etiquetas-de-órdenes-de-compra)
6. [Códigos de Organismos y Proveedores](#códigos-de-organismos-y-proveedores)
7. [Ejemplos Prácticos](#ejemplos-prácticos)
8. [Notas Importantes](#notas-importantes)

---

## Características Generales

La API utiliza el método **HTTP GET** para realizar consultas, permitiendo especificar parámetros directamente en la URL. Los datos están disponibles en tres formatos: **JSON**, **JSONP** y **XML**. Cada consulta requiere un **ticket** de autenticación, y las fechas deben seguir el formato `ddmmaaaa` (día, mes, año sin separadores).

- **Base URL**: `https://api.mercadopublico.cl/servicios/v1/publico/`
- **Endpoints principales**:
  - Licitaciones: `licitaciones`
  - Órdenes de Compra: `ordenesdecompra`
  - Códigos de Proveedores/Organismos: `Empresas/BuscarProveedor` y `Empresas/BuscarComprador`

---

## Autenticación

Para usar la API, necesitas un **ticket**. Este es un identificador único que autentica tus solicitudes. En esta documentación usamos un ticket de prueba:

- **Ticket de prueba**: `F8537A18-6766-4DEF-9E59-426B4FEE2844`
- **Nota**: Este ticket es solo para pruebas. Para un uso real, solicita tu propio ticket en la plataforma oficial de Mercado Público.

El ticket se incluye como parámetro en todas las URLs: `&ticket=F8537A18-6766-4DEF-9E59-426B4FEE2844`.

---

## Formatos de Respuesta

La API soporta tres formatos de salida:

1. **JSON**: Ideal para aplicaciones modernas.
   - Ejemplo: `.json?fecha=02022014&ticket=...`
2. **JSONP**: Para uso en scripts con callbacks.
   - Ejemplo: `.jsonp?fecha=02022014&callback=respuesta&ticket=...`
3. **XML**: Formato estructurado clásico.
   - Ejemplo: `.xml?fecha=02022014&ticket=...`

---

## Consultas de Licitaciones

### Tipos de Consultas (Licitaciones)

El endpoint base es `https://api.mercadopublico.cl/servicios/v1/publico/licitaciones`. Puedes aplicar los siguientes filtros:

1. **Por Código de Licitación**
   - Ejemplo: `?codigo=1509-5-L114`
   - URL: `https://api.mercadopublico.cl/servicios/v1/publico/licitaciones.json?codigo=1509-5-L114&ticket=F8537A18-6766-4DEF-9E59-426B4FEE2844`
   - Resultado: Detalles completos de la licitación.

2. **Por Día Actual (Todos los Estados)**
   - URL: `https://api.mercadopublico.cl/servicios/v1/publico/licitaciones.json?ticket=F8537A18-6766-4DEF-9E59-426B4FEE2844`

3. **Por Fecha Específica**
   - Ejemplo: `?fecha=02022014`
   - URL: `https://api.mercadopublico.cl/servicios/v1/publico/licitaciones.json?fecha=02022014&ticket=F8537A18-6766-4DEF-9E59-426B4FEE2844`

4. **Por Estado "Activas"**
   - URL: `https://api.mercadopublico.cl/servicios/v1/publico/licitaciones.json?estado=activas&ticket=F8537A18-6766-4DEF-9E59-426B4FEE2844`

5. **Por Estado y Fecha**
   - Ejemplo: `?fecha=02022014&estado=adjudicada`
   - URL: `https://api.mercadopublico.cl/servicios/v1/publico/licitaciones.json?fecha=02022014&estado=adjudicada&ticket=F8537A18-6766-4DEF-9E59-426B4FEE2844`

6. **Por Código de Organismo Público**
   - Ejemplo: `?fecha=02022014&CodigoOrganismo=6945`
   - URL: `https://api.mercadopublico.cl/servicios/v1/publico/licitaciones.json?fecha=02022014&CodigoOrganismo=6945&ticket=F8537A18-6766-4DEF-9E59-426B4FEE2844`

7. **Por Código de Proveedor**
   - Ejemplo: `?fecha=02022014&CodigoProveedor=17793`
   - URL: `https://api.mercadopublico.cl/servicios/v1/publico/licitaciones.json?fecha=02022014&CodigoProveedor=17793&ticket=F8537A18-6766-4DEF-9E59-426B4FEE2844`

### Estados de Licitaciones

Los estados se representan con códigos:

- `5`: Publicada
- `6`: Cerrada
- `7`: Desierta
- `8`: Adjudicada
- `18`: Revocada
- `19`: Suspendida

En las URLs, puedes usar los nombres: `publicada`, `cerrada`, `desierta`, `adjudicada`, `revocada`, `suspendida`, `todos`.

### Etiquetas de Licitaciones

Los datos devueltos incluyen:

- **Cantidad**: Número de licitaciones.
- **FechaCreacion**: Fecha de la consulta.
- **Version**: Versión de la API.
- **Listado**: Lista de licitaciones.
- **Tipo de Licitación**: Ejemplo: `L1` (Menor a 100 UTM), `LE` (100-1000 UTM), `LP` (Mayor a 1000 UTM).
- **Unidad Monetaria**: `CLP`, `USD`, `UTM`, etc.
- **Monto Estimado**: `1` (Presupuesto), `2` (Referencial).
- **Modalidad de Pago**: `1` (30 días), `3` (Al día), etc.
- **Valores Binarios**: `1` (Sí) o `0` (No) para campos como `<Informada>`, `<SubContratacion>`, etc.

---

## Consultas de Órdenes de Compra

### Tipos de Consultas (Órdenes de Compra)

El endpoint base es `https://api.mercadopublico.cl/servicios/v1/publico/ordenesdecompra`. Filtros disponibles:

1. **Por Código de Orden**
   - Ejemplo: `?codigo=2097-241-SE14`
   - URL: `https://api.mercadopublico.cl/servicios/v1/publico/ordenesdecompra.json?codigo=2097-241-SE14&ticket=F8537A18-6766-4DEF-9E59-426B4FEE2844`

2. **Por Día Actual (Todos los Estados)**
   - URL: `https://api.mercadopublico.cl/servicios/v1/publico/ordenesdecompra.json?estado=todos&ticket=F8537A18-6766-4DEF-9E59-426B4FEE2844`

3. **Por Fecha Específica**
   - Ejemplo: `?fecha=02022014`
   - URL: `https://api.mercadopublico.cl/servicios/v1/publico/ordenesdecompra.json?fecha=02022014&ticket=F8537A18-6766-4DEF-9E59-426B4FEE2844`

4. **Por Estado y Fecha**
   - Ejemplo: `?fecha=02022014&estado=aceptada`
   - URL: `https://api.mercadopublico.cl/servicios/v1/publico/ordenesdecompra.json?fecha=02022014&estado=aceptada&ticket=F8537A18-6766-4DEF-9E59-426B4FEE2844`

5. **Por Código de Organismo Público**
   - Ejemplo: `?fecha=02022014&CodigoOrganismo=6945`
   - URL: `https://api.mercadopublico.cl/servicios/v1/publico/ordenesdecompra.json?fecha=02022014&CodigoOrganismo=6945&ticket=F8537A18-6766-4DEF-9E59-426B4FEE2844`

6. **Por Código de Proveedor**
   - Ejemplo: `?fecha=02022014&CodigoProveedor=17793`
   - URL: `https://api.mercadopublico.cl/servicios/v1/publico/ordenesdecompra.json?fecha=02022014&CodigoProveedor=17793&ticket=F8537A18-6766-4DEF-9E59-426B4FEE2844`

### Estados de Órdenes de Compra

Los estados tienen códigos y nombres para las URLs:

- `4` (`enviadaproveedor`): Enviada a Proveedor
- `5`: En proceso
- `6` (`aceptada`): Aceptada
- `9` (`cancelada`): Cancelada
- `12` (`recepcionconforme`): Recepción Conforme
- `13` (`pendienterecepcion`): Pendiente de Recepcionar
- `14` (`recepcionaceptadacialmente`): Recepcionada Parcialmente
- `15` (`recepecionconformeincompleta`): Recepción Conforme Incompleta

### Etiquetas de Órdenes de Compra

Los datos devueltos incluyen:

- **Cantidad**: Número de órdenes.
- **FechaCreacion**: Fecha de la consulta.
- **Version**: Versión de la API.
- **Listado**: Lista de órdenes.
- **Tipo de Orden**: Ejemplo: `1` (Automática), `2` (Trato Directo Proveedor Único), `9` (Convenio Marco).
- **Unidad Monetaria**: `CLP`, `USD`, `UTM`, etc.
- **Tipo de Despacho**: `7` (A dirección), `14` (Retiro en bodega), etc.
- **Tipo de Pago**: `1` (15 días), `2` (30 días), etc.

---

## Códigos de Organismos y Proveedores

### Buscar Proveedor

Obtén el código de un proveedor usando su RUT.

- **URL**: `https://api.mercadopublico.cl/servicios/v1/Publico/Empresas/BuscarProveedor`
- **Ejemplo**:

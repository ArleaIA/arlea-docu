# Normalización de Datos
## Descripción

Este documento describe el proceso de normalización de datos aplicado por el Worker antes de persistir información en la base de datos.
La normalización tiene como objetivo transformar datos crudos provenientes del scrapper en un formato consistente, predecible y compatible con el modelo interno del sistema.

El proceso se ejecuta de forma determinística y desacoplada de la persistencia, reduciendo errores aguas abajo y asegurando integridad de los datos almacenados.

## Objetivo

- Transformar payloads heterogéneos en estructuras coherentes.
- Validar información mínima antes de persistencia.
- Reducir duplicidad y ruido semántico.
- Preparar datos listos para transacciones SQL.
- Separar responsabilidades entre transformación y almacenamiento.

## Proceso de Normalización
**Validación Inicial**

En esta etapa se verifica que el payload recibido desde Redis cumpla con una estructura mínima válida.
Validaciones realizadas:
- Existencia de campos obligatorios.
- Presencia de identificadores mínimos del producto.
- Tipos de datos compatibles con el modelo interno.
- Estructura general del objeto.

Si el payload no cumple estas condiciones, el procesamiento del mensaje se detiene.

## Limpieza de Datos

Una vez validada la estructura, se aplican transformaciones básicas para eliminar inconsistencias comunes del scrapper.
Acciones realizadas:

- Eliminación de espacios innecesarios.
- Normalización de texto (mayúsculas y minúsculas).
- Limpieza de caracteres especiales no relevantes.
- Conversión de valores vacíos o indefinidos.

Esta etapa reduce el ruido y homogeniza los datos.

## Estandarización

Los datos limpios se mapean al modelo interno del Worker.
Procesos incluidos:

- Normalización del nombre del producto.
- Normalización de la marca.
- Estandarización del tipo de producto.
- Preparación de estructuras de colores.
- Separación explícita entre atributos del producto y atributos dependientes de tienda.

El resultado es un objeto con estructura predecible.

## Reglas de Negocio

Durante la normalización se aplican reglas propias del dominio:

- Separación entre producto y relación producto–tienda.
- Preparación de datos reutilizables entre tiendas.
- Prevención de duplicados a nivel lógico.
- Resolución de conflictos simples provenientes del scrapper.
- Estas reglas permiten coherencia en la persistencia posterior.

## Resultado de la Normalización

El proceso de normalización produce:

- Datos consistentes y coherentes.
- Estructuras compatibles con la capa de persistencia.
- Reducción significativa de errores de base de datos.
- Separación clara entre lógica de transformación y persistencia.

## Consideraciones técnicas

- El normalizador es un componente puro.
- No accede a Redis ni a la base de datos.
- No mantiene estado interno.
- No ejecuta operaciones con efectos secundarios.
- Está diseñado para ser fácilmente testeable.

## Observaciones

- El proceso puede extenderse con nuevas reglas sin afectar la persistencia.
- Permite incorporar nuevas fuentes de datos de forma controlada.
- Es clave para garantizar calidad de datos en el sistema.
- Facilita debugging al aislar errores antes de la transacción.
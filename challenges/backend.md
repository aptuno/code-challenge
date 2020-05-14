# Desafio para Backend Software Engineer

## Descripción del problema

En Aptuno trabajamos con `propriedades` de propietarios que están arrendando su inmueble con nosotros. Esas `propiedades` están localizadas en una o más `regiones` de la ciudad; el objetivo de este desafío es realizar una *API* que permita realizar algunas operaciones de *CRUD* para cada una de esas dos entidades con algunas reglas de negocio sobre ellas.

### Regiones

Para este ejercicio vamos a simplificar el concepto de *región*, para nuestro desafío, va a ser representada como un [BoundingBox](https://wiki.openstreetmap.org/wiki/Bounding_Box) dentro la ciudad. 

Como ejemplos de región, podemos representar algunos barrios, áreas o localidades populares de la ciudad de Bogotá, como por ejemplo [La Candelaria](http://bboxfinder.com/#4.587403,-74.083511,4.604129,-74.058277), [Chapinero](http://bboxfinder.com/#4.621519,-74.069225,4.681916,-74.024078), [Marly](http://bboxfinder.com/#4.625633,-74.069253,4.640647,-74.062644), [Zona T](http://bboxfinder.com/#4.663527,-74.057316,4.670970,-74.051093), [La Floresta](http://bboxfinder.com/#4.685201,-74.076193,4.696663,-74.068211), entre otros. Note que en los ejemplos anteriores tenemos diferentes escenarios de regiones; regiones aisladas de las otras regiones, regiones que tienen sobreposición en algunas cuadras con otra región, regiones que están contenidas en otras regiones.

#### Representación de una región

A continuación un ejemplo de la representación *JSON* de una región, por ejemplo para representar la región de *Chapinero*:

```json
{
  "id": 1,
  "name": "Chapinero",
  "boundingBox": {
    "bottomLeft" : {
        "longitude" : -74.069225,
        "latitude" : 4.621519
      },
      "upperRight" : {
        "longitude" : -74.024078,
        "latitude" : 4.681916
      }
    }
  }
}
```

#### Reglas sobre regiones

1. Los *ids* deben ser numéricos generados automáticamente a partir del número 1.
2. El nombre no debe ser vacío y debe ser único. 
3. Los valores del *BoundingBox* no pueden ser nulos, y tanto las latitudes como las longitudes deben ser validadas dentro de sus rangos de valores permitidos para este tipo de datos.
4. Las propiedades en el *BoundingBox* son incluyentes de las regiones, por lo tanto si una propiedad está registrada en una de las esquinas del *BoundingBox*, esa propiedad también hace parte de la región.

#### Desafio

Usando las siguientes estructuras de `Request` y `Body` permita las siguientes funcionalidades. Para crear o alterar regiones, tenga en cuenta que los campos son obligatorios y deben respetar las reglas descritas para anteriormente.

1. **Cree nuevas regiones.** 

Para la persistencia de las regiones, utilice cualquier repositorio de su preferencia (*SQL* o *NoSQL*).

*Request*:
```
POST /regions
```

*Body*:
```json
{
  "name": "Chapinero",
  "boundingBox": {
    "bottomLeft" : {
        "longitude" : -74.069,
        "latitude" : 4.621
      },
      "upperRight" : {
        "longitude" : -74.024,
        "latitude" : 4.681
      }
    }
  }
}
```

*Response*:

Usted define la respuesta, hace parte del desafío


2. **Actualice una región**

*Request*:
```
PUT /regions/{id}
```

*Body*:
```json
{
  "id": 1,
  "name": "Chapinero",
  "boundingBox": {
    "bottomLeft" : {
        "longitude" : -74.069225,
        "latitude" : 4.621519
      },
      "upperRight" : {
        "longitude" : -74.024078,
        "latitude" : 4.681916
      }
    }
  }
}
```

*Response*:

Usted define la respuesta, hace parte del desafío


3. **Retorne una región específica** 

*Request*:
```
GET /regions/{id}
```

*Response*:

```json
{
  "id": 1,
  "name": "Chapinero",
  "boundingBox": {
    "bottomLeft" : {
        "longitude" : -74.069225,
        "latitude" : 4.621519
      },
      "upperRight" : {
        "longitude" : -74.024078,
        "latitude" : 4.681916
      }
    }
  }
}
```

4. **Retorne las propiedades de una región específica** 

Retorna las propiedades de una región. Los resultados vienen páginados y organizados por fecha de actualización del más reciente al más antiguo. Si no tiene fecha de actualización, debe usar la fecha de creación para ordenar.

*Request*:
```
GET /regions/{id}/properties?page={pageNumber}&pageSize={pageSize}
```
Donde:
  - `page`: Es el número de la página; es un parámetro opcional y su valor default es 1
  - `pageSize`: Es el tamaño solicitado de resultados en la página. Es un parámetro opcional, su valor default es 10, y su valor máximo es 20.
  
*Response*:
La respuesta debe seguir la siguiente estructura de campos:

  - `page`: La página actual de los resultados
  - `pageSize`: El máximo número de resultados retornado por página
  - `total`: El número total de propriedades en la región
  - `totalPages`: El número total de páginas que contienen resultados para la búsqueda hecha.
  - `data`: Un array con los objetos conteniendo las propiedades solicitadas en el request. Para ver en detalle la estructura de una `propiedad`, por favor avance a la siguiente sección.

```json
{
  "page": 1,
  "pageSize": 10,
  "totalPages": 1,
  "total": 1,
  "data": [
    {
      "id": 10,
      "title": "Apartamento cerca a la estación de transmilenio",
      "description": "Apartamento con 3 cuartos y 2 baños localizado a 100 metros de la avenida caracas en la zona de Marly",
      "location": {
        "longitude" : -74.0652887,
        "latitude" : 4.6370493
      },
      "pricing": {
        "rentalPrice": 2000000,
        "administrativeFee": 250000
      },
      "bedrooms": 3,
      "bathrooms": 2,
      "area": 60,
      "photos": [
        "https://cdn.pixabay.com/photo/2014/08/11/21/39/wall-416060_960_720.jpg", 
        "https://cdn.pixabay.com/photo/2016/09/22/11/55/kitchen-1687121_960_720.jpg"
      ],
      "createdAt": "2020-05-10T04:20:33Z",
      "updatedAt": "2020-05-10T05:30:00Z",
      "regions": ["Chapinero", "Marly"]
    }
  ]
}
```


### Propiedades

Una propiedad tiene la información básica para anunciar un inmueble. La propiedad debe estar asociada en una o más regiones.


#### Representación de una propiedad

A continuación un ejemplo de la representación *JSON* de una propiedad, por ejemplo para representar una propiedad en la región de *Marly*:

```json
{
  "id": 10,
  "title": "Apartamento cerca a la estación de transmilenio",
  "description": "Apartamento con 3 cuartos y 2 baños localizado a 100 metros de la avenida caracas en la zona de Marly",
  "location": {
    "longitude" : -74.0652887,
    "latitude" : 4.6370493
  },
  "pricing": {
    "rentalPrice": 2000000,
    "administrativeFee": 250000
  },
  "bedrooms": 3,
  "bathrooms": 2,
  "area": 60,
  "photos": [
    "https://cdn.pixabay.com/photo/2014/08/11/21/39/wall-416060_960_720.jpg", 
    "https://cdn.pixabay.com/photo/2016/09/22/11/55/kitchen-1687121_960_720.jpg"
  ],
  "createdAt": "2020-05-10T04:20:33Z",
  "updatedAt": "2020-05-10T05:30:00Z",
  "regions": ["Chapinero", "Marly"]
}
```

#### Reglas sobre propriedades

1. Los *ids* deben ser numéricos generados automáticamente a partir del número 1.
2. Los campos `title`, `location`, `pricing.rentalPrice`, `bedrooms`, `bathrooms`, `area` son obligatórios.
3. Los campos `createdAt`, `updatedAt` y `regions` son automáticamente generados por el sistema. La API não debería permitir modificaciones sobre ellos.
4. El campo `bedrooms` tiene como valor mínimo 1 y valor máximo 6.
5. El campo `bathrooms` tiene como valor mínimo 1 y valor máximo 4.
6. El campo `area` tiene como valor mínimo 15 y valor máximo 300.
7. La localización de la propiedad debe corresponder a un punto dentro de por lo menos una `región` registrada previamente en la aplicación.

#### Desafio

Usando las siguientes estructuras de `Request` y `Body` permita las siguientes funcionalidades. Para crear o alterar propiedades, tenga en cuenta las reglas descritas anteriormente. 

Un punto importante es que para las `propriedades` no debe ser usada una persistencia externa; las `propiedades` deben ser almacenadas en memoria dentro de la aplicación, usando alguna de las estructuras de datos del lenguaje de programación que usted seleccionó.

1. **Cree nuevas propriedades.** 

*Request*:
```
POST /properties
```

*Body*:
```json
{
  "title": "Apartamento cerca a la estación de transmilenio",
  "location": {
    "longitude" : -74.0652887,
    "latitude" : 4.6370493
  },
  "pricing": {
    "rentalPrice": 2000000,
  },
  "bedrooms": 3,
  "bathrooms": 2,
  "area": 60
}
```
*Response*:

Usted define la respuesta, hace parte del desafío

2. **Actualice una propiedad**

*Request*:
```
PUT /properties/{id}
```

*Body*:
```json
{
  "id": 10,
  "title": "Apartamento cerca a la estación de transmilenio",
  "description": "Apartamento con 3 cuartos y 2 baños localizado a 100 metros de la avenida caracas en la zona de Marly",
  "location": {
    "longitude" : -74.0652887,
    "latitude" : 4.6370493
  },
  "pricing": {
    "rentalPrice": 2000000,
    "administrativeFee": 250000
  },
  "bedrooms": 3,
  "bathrooms": 2,
  "area": 60,
  "photos": [
    "https://cdn.pixabay.com/photo/2014/08/11/21/39/wall-416060_960_720.jpg", 
    "https://cdn.pixabay.com/photo/2016/09/22/11/55/kitchen-1687121_960_720.jpg"
  ]
}
```

*Response*:

Usted define la respuesta, hace parte del desafío


3. **Retorne propiedades** 

Retorna las propiedades filtradas por un *BoundingBox* (opcional). Los resultados vienen páginados y organizados por fecha de actualización del más reciente al más antiguo. Si no tiene fecha de actualización, debe usar la fecha de creación para ordenar.

*Request*:
```
GET /properties?bbox={bbox}&page={pageNumber}&pageSize={pageSize}
```
Donde:
  - `bbox`: Es un [BoundingBox](https://wiki.openstreetmap.org/wiki/Bounding_Box) para filtrar propiedades. Es un parámetro opcional, su valor default es `null`, es decir debe retornar todas las propiedades. El formato del parámetro esperado en la url es `bbox={minLongitude},{minLatitude},{maxLongitude},{maxLatitude}`. 
  - `page`: Es el número de la página; es un parámetro opcional y su valor default es 1
  - `pageSize`: Es el tamaño solicitado de resultados en la página. Es un parámetro opcional, su valor default es 10, y su valor máximo es 20.

*Response*:
La respuesta debe seguir la siguiente estructura de campos:

  - `page`: La página actual de los resultados
  - `pageSize`: El máximo número de resultados retornado por página
  - `total`: El número total de propriedades
  - `totalPages`: El número total de páginas que contienen resultados para la búsqueda hecha.
  - `data`: Un array con los objetos conteniendo las propiedades solicitadas en el request. Para ver en detalle la estructura de una `propiedad`, por favor avance a la siguiente sección.

```json
{
  "page": 1,
  "pageSize": 10,
  "totalPages": 1,
  "total": 1,
  "data": [
    {
      "id": 10,
      "title": "Apartamento cerca a la estación de transmilenio",
      "description": "Apartamento con 3 cuartos y 2 baños localizado a 100 metros de la avenida caracas en la zona de Marly",
      "location": {
        "longitude" : -74.0652887,
        "latitude" : 4.6370493
      },
      "pricing": {
        "rentalPrice": 2000000,
        "administrativeFee": 250000
      },
      "bedrooms": 3,
      "bathrooms": 2,
      "area": 60,
      "photos": [
        "https://cdn.pixabay.com/photo/2014/08/11/21/39/wall-416060_960_720.jpg", 
        "https://cdn.pixabay.com/photo/2016/09/22/11/55/kitchen-1687121_960_720.jpg"
      ],
      "createdAt": "2020-05-10T04:20:33Z",
      "updatedAt": "2020-05-10T05:30:00Z",
      "regions": ["Chapinero", "Marly"]
    }
  ]
}
```

## Entrega del proyecto
Para revisitar los criterios de calificación del desafío, y la descripción de cómo entregar el proyecto, por favor continúe leyendo la documentación principal a partir de [aquí](../README.md#critérios-de-calificación).

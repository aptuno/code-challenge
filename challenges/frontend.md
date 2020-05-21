# Desafio para Frontend Software Engineer

## Descripción del problema

En Aptuno estamos pensando en restructurar una nueva página de resultados de nuestros inmuebles y una página de detalle de un inmueble. Para eso queremos crear una versión simple y reducida de como podríamos restructurar esas páginas.

#### Desafio

A partir de una lista de propiedades que vamos a disponibilizar en el siguiente [archivo](https://raw.githubusercontent.com/aptuno/code-challenge/master/challenges/data/properties.json), por favor cree una página de resultados y una página de detalle que cumpla con los requerimientos definidos. El archivo tiene el siguiente formato.

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
      "type": "APARTMENT",
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

1. **Cree una página de resultado.** 

##### Card

Cree una versión simplificada de una página de resultados (puede ser en formato `list` o en formato `grid`). Para cada resultado de las propiedades, debe cumplir con los siguientes requisitos.

- Debe ser responsiva.
- La propiedad listada debe mostrar los siguientes atributos: Fotos, título, tipo de la propiedad, # de cuartos, área, y el precio total (precio del arriendo + condominio).
- Deben aparecer todas las fotos usando un carrusel de imágenes.
- Debe tener una acción que permita navegar a la página de detalle de la propriedad.

##### Filtros

La página de resultados debe permitir filtrar los siguientes atributos:

- Región. Deje disponible un dropdown con todas las regiones disponibles en el JSON de propriedades. Cuando seleccione una región, filtre y deje disponibles solo las propriedades que están dentro de la región seleccionada.
- Cuartos. Puede ser una serie de checkbox con los números de cuartos que el usuario quiere filtrar. Por ejemplo puedo buscar propriedades con 2 o 3 cuartos al tiempo.

##### Paginación

La página de resultados debe tener una paginación hecha por usted, debe destacar la página actual de las otras páginas. Tenga en cuenta la cantidad de resultados filtrados al ajustar su paginación. Cuando el usuario filtra resultados, va a quedar activa por *default* la página 1.

Cada página debe retornar máximo 12 propriedades.

2. **Cree una página de detalle.** 

Usando la información disponible de una propiedad, cree una versión responsiva de cómo usted cree que deberíamos representar un inmueble. Presente todos los atributos que fueron retornados para la propiedad.

## Entrega del proyecto
Para revisitar los criterios de calificación del desafío, y la descripción de cómo entregar el proyecto, por favor continúe leyendo la documentación principal a partir de [aquí](../README.md#critérios-de-calificación).
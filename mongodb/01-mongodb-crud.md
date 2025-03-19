# CRUD y Consultas en MongoDB

## Crear una Base de Datos
Solo se crea si contiene por lo menos una colección

**use bd1** 

## Como crear una colección
```json
use bd1
db.createCollection("Empleado")
```
        
## Mostrar las colecciones 
```json
show collections
```

## Insertar un Documento
```json
db.alumnos.insertOne(
{
   nombre: 'Soyla',
   apellido1: 'Vaca',
   edad: 32,
   ciudad: 'San Miguel de las Piedras'
}
)
```

## Inserción de un documento más complejo con array
```json
db.alumnos.insertOne(
{
  nombre: "Joquin",
  apellido: "Dorian",
  apellido2: "Guerrero",
  edad: 15,
  aficiones: [ 'Cerveza', 'Hueva', 'Canabis' ]
})
```

## Inserción de documentos más complejos con documentos anidados y ID
```json
db.alumnos.insertOne(
{
      nombre: 'Jose Luis',
      apellido1: "Herrera",
      apellido2: "Gallardo",
      edad: 41,
      estudios: [
               'Ing en Sistemas Computacionales',
               'Maestria en Tecnologías de Información'
             ],
  experiencia: {
                 lenguaje: 'SQL',
                 sbd: "SQL Server",
                 aniosExp: 14
              }
}
)

db.alumnos.insertOne({
    _id: 3, 
    nombre: 'Sergio', 
    apellido: 'Ramos', 
    equipo: 'Monterrey', 
    aficiones: [ 'Dinero', 'Hombres', 'Fiesta' ], 
    talentos: {
        futbol: true, 
        bañarse: false
    }
}
)
```

## Insertar Múltiples documentos
```json
db.alumnos.insertMany(
   [
     {
         _id: 12,
         nombre: 'Roberto',
         apellido: 'Gomez',
         edad: "23",
         descripcion: "Es un comediante bueno"
     },
     {
         nombre: 'Luis',
         apellido: "Suarez",
         edad: 43,
         habilidades: [
                       'Correr', 'dormir', 'Morder'
                      ],
        direcciones: {
                         calle: 'Del infierno',
                         numero: 666
                     },
        esposas: [
                     {
                   nombre: "Marisol",
                   edad: 20,
                   pension: 350
                   , hijos: ['Joaquin', 'Bridget']
                     },
                    {
                       nombre: "Dorien",
                       edad: 46,
                       pension: 6500.56,
                       complaciente: true
                    }
                ]
  }
]
)
```

# Práctica 1

## Cargar Datos
[Libros.json](./data/libros.json)

## Búsquedas. Condiciones Simples de Igualdad. Método find()

1. Seleccionar todos los documentos de la colección libros
```json
db.libros.find({})
```

2. Mostrar todos los documentos que sean de la editorial biblio
```json
db.libros.find({editorial:'Biblio'})
```

3. Mostrar todos los documentos cuyo precio sea 25
```json
db.libros.find({precio:25})
```

4. Seleccionar todos los documentos donde el título sea "JSON para todos"
```json
db.libros.find({titulo:'JSON para todos'})
```

## Operadores de Comparación
[Operadores de comparación](https://www.mongodb.com/docs/manual/reference/operator/query/)

![Operadores de Comparación](/img/image.png)

1. Mostrar todos los documentos donde el precio sea mayor a 25
```json
db.libros.find({ precio: { $gt: 25 } })
```

2. Mostrar los documentos donde el precio sea 25
```json
db.libros.find({ precio: { $eq: 25 } })
```

3. Mostrar los documentos cuya cantidad sea menor a 5
```json
db.libros.find({ cantidad: { $lt: 5 } })
```

4. Mostrar los documentos que pertenezcan a la editorial "Biblio" o "Planeta"
```json
db.libros.find({editorial:{$in:['Biblio', 'Planeta']}})
```

5. Mostrar todos los documentos de libros que cuesten 20 o 25
```json
db.libros.find({precio:{$in:[20, 25]}})
```

6. Mostrar todos los documentos de libros que no cuesten 20 o 25
```json
db.libros.find({precio:{$nin:[20, 25]}})
```

7. Mostrar el primer documento de libros que cueste 20 o 25
```json
db.libros.findOne({ precio: { $in: [20, 25] } })
```

## Operadores Lógicos
[Operadores Lógicos](https://www.mongodb.com/docs/manual/reference/operator/query/)

![Operadores Lógicos](/img/OperadoresLogicos.png)

### Operador AND

 1. Dos posibles opciones de AND
 
 1. La simple, mediante condiciones separadas por comas
 
 ***sintaxis***
 
 db.coleccion.find({condicion1, condicion2}) -> Con esto asume que es una ***and***
 
 2. Usando el operador $and
 
 ***sintaxis***
 db.coleccion.find({$and:[{condicion1},{condicion2}]})
 

#### Ejercicios

1. Mostrar todos aquellos libros que cuesten más de 25 y cuya cantidad sea inferior a 15
```json
db.libros.find({ precio: { $gt: 25 }, cantidad: { $lt: 15 } })
```

2. Mostrar todos aquellos libros que cuesten más de 25, cuya cantidad sea inferior a 15 y el id igual a 4
```json
db.libros.find( { precio: { $gt: 25 }, cantidad: { $lt: 15 }, _id:4})
```

### Operador OR

1. Mostrar todos aquellos libros que cuesten más de 25 o cuya cantidad sea inferior a 15
```json
db.libros.find( { $or: [{ precio: { $gt: 25 } }, { cantidad: { $lt: 15 } }] })
```

2. Mostrar los libros de la editorial "Biblio" con precio mayor a 40 o libros de la editorial "Planeta" con precio mayor a 30
```json
db.libros.find(
    {
      $or: [
           { $and:[{editorial:'Biblio'},{precio:{$gt:30}}]},
           { $and:[{editorial:{$eq:'Planeta'}},{precio:{$gt:20}}]}
      ]
    }
)
```

### AND y OR Combinadas 
 
 1. Mostrar los libros de la editorial Biblio con precio mayor a 40 o libros de la editorial Planeta
    con precio mayor a 30
      
 ```json
 db.libros.find(
     {
       $or: [
            { $and:[{editorial:'Biblio'},{precio:{$gt:30}}]},
            { $and:[{editorial:{$eq:'Planeta'}},{precio:{$gt:20}}]}
       ]
     }
 )
 ```
 
 ```json
 db.libros.find(
     {
       $or: [
            { $and:[{editorial:'Biblio'},{precio:{$gt:30}}]},
            { $and:[{editorial:{$eq:'Planeta'}},{precio:{$gt:20}}]}
       ]
     }
 )
 ```
 ```json
 db.libros.find(
     {
       $or: [
            {editorial:'Biblio',precio:{$gt:30}},
            { editorial:{$eq:'Planeta'},precio:{$gt:20}}
       ]
     }
 )
 ```
 
## Proyección de Columnas
### Sintaxis
```json
db.coleccion.find(filtro, columnas)
```

1. Seleccionar todos los documentos, mostrando el título y la editorial
```json
db.libros.find({},{titulo:1, editorial:1})
```

2. Seleccionar todos los documentos de la editorial Planeta, mostrando solamente el título y la editorial
```json
db.libros.find({editorial:'Planeta'}, {_id:0, titulo:1, editorial:1})
```

## Operador exists (Permite saber si un campo se encuentra o no en un documento)
```json
db.libros.find({ editorial:{$exists:true}})
```

1. Mostrar todos los documentos que no contengan el campo cantidad
```json
db.libros.find( { cantidad: { $exists: true }})
```

## Operador Type (Permite preguntar si un determinado campo corresponde con un tipo)
 
 [Operador Type](https://www.mongodb.com/docs/manual/reference/operator/query/type/#mongodb-query-op.-type)
 
 1. Mostrar todos los documentos donde el precio sean dobles
 
 ```json
 db.libros.find({precio:{$type:1}})
 ```
 ```json
 db.libros.find({precio:{$type:16}})
 ```
 
 ```json
 db.libros.insertOne({
   _id:11,
   titulo:'IA',
   editorial: 'Terra',
   precio:125.4,
   cantidad:20
 })
 ```
 
  ```json
  db.libros.find({precio:{$type:1}}, {_id:0})
  ```
 
  ```json
  db.libros.find({precio:{$type:1}}, {_id:0, cantidad:0})
  ```
 
  ```json
   db.libros.insertMany([
  {
     _id: 12,
     titulo: 'IA',
     editorial: 'Terra',
     precio: 125, 
 	cantidad: 20
   },
   {
     _id: 13,
     titulo: 'Python para todos',
     editorial: 2001,
     precio: 200, 
 	cantidad: 30
   }]
   )
   ```
 
 1. Seleccionar los documentos donde la ediatorial sea de tipo entero
  ```json
   db.libros.find({editorial:{$type:16}})
   ```
  ```json
   db.libros.find({editorial:{$type:'int'}})
   ```
 
 2. Seleccionar todos los documentos  donde la editorial sea string
 ```json
 db.libros.find({editorial:{$type:'string'}})
 ```
```json
 db.libros.find({editorial:{$type:2}})
 ```

 # Modificando Documentos
 ## Comandos importantes
 
 1. updateOne -> Modificar un solo documento
 2. updateMany -> Modificar multiples documentos
 3. replaceOne -> Sustituir el contenido completo de un documento
 
 Tiene el siguiente formato:
 
 ``` json
 db.collection.updateOne(
   {filtro},
   {operador: }
 )
 ```
 
 [Operadores Update](https://www.mongodb.com/docs/manual/reference/operator/update/)
 
 ### Operador set
 
 1. Modificar un documento
 
 ``` json
 db.libros.updateOne({titulo:'Python para todos'},{$set:{titulo:'Java para Todos'}})
 ```
 
 db.libros.updateOne({_id:2},{$set:{precio:56, existencia:10}})
 
 
 2. Actualizar el precio a 100 y la cantidad a 50 para el _id:10
 
 db.libros.updateOne({_id:10},{$set:{precio:100, cantidad:50}})
 
 #### Modificar Multiples Documentos
 
 -- Modificar todos los documentos donde el precio sea mayor a 100 a un precio de 150
 
 ``` json
 db.libros.updateMany(
   {precio:{$gt:100}},
   {$set:{precio:150}}
 )
 ```
 2. Operador $inc y $mul
 
 - Actualizar con un incremento de cinco todos los documentos
```json
 db.libros.updateMany(
   {},
   {$inc:{precio:5}}
 )
```
 - Actualizar con multiplicación de 2 todos los documentos que la cantidad sean mayores a 20
 ```json
 db.libros.updateMany({cantidad:{$gt:20}},{$mul:{cantidad:2}})
 ```
 - Actualizar todos los documentos donde el precio sea mayor a 20 y se multiplique por 2 la cantidad y el precio
```json
 db.libros.updateMany({precio:{$gt:20}},{$mul:{cantidad:2, precio:2}})
```
 3. Reemplazar Documentos (replaceOne)
 
```json
 db.libros.replaceOne({_id:2}, {titulo:'De la Tierra a la Luna', autor:'Julio Verne', precio:500})
```
 
 # Borrar Documentos
 
 1. deleteOne -> Elimina un solo documento
 2. deleteMany -> Elimina Multiples documentos
 
 1. Eliminar el documento con id 2
 ```json
 db.libros.deleteOne({_id:2})
 ```
 2. Eliminar los documentos donde la cantidad sea mayor o igual a 150
```json
 db.libros.deleteMany({cantidad:{$gte:100}})
 ```
 # Expresiones Regulares 
 
 1. Buscar los libros que contenga el titulo la letra t
```json 
 db.libros.find({titulo:/t/})
``` 
 2. Buscar los libros que en el titulo contengan la palabra json
 ```json
 db.libros.find({titulo:/JSON/})
  ```
 3. Buscar todos los documentos que en titulo terminen en tos
 ```json
 db.libros.find({titulo:/tos$/})
 ```
 4. Todos los documentos que en el titulo comiencen con J
 ```json
 db.libros.find({titulo:/^J/})
 ```
 # Operador $regex
 [Operador Regex](https://www.mongodb.com/docs/manual/reference/operator/query/regex/)
 
 - Seleccionar los libros que contengan la palabra para en titulo
```json
 db.libros.find({titulo: {$regex: 'para'}})
``` 
```json 
 db.libros.find({titulo:{$regex:'JSON'}})
```
 ```json
 db.libros.find({titulo:{$regex:/JSON/}})
 ```
 - Distinguir entre mayusculas y minusculas
```json 
 db.libros.find({titulo:{$regex:/json/}}) -> No distingue entre mayusculas y minusculas
``` 
```json
 db.libros.find({titulo:{$regex:/json/i}})
```
```json
 db.getCollection('libros').find({
   titulo: {
     $regex: RegExp('json'),
     $options: 'i'
   }
 });
 ```
 -- Seleccionar todos los libros que comiencen con j o J
```json 
 db.getCollection('libros').find({
   titulo: { $regex: RegExp('^j', 'i') }
 });
 ```
 --seleccionar todos los libros que terminen es
```json  
 db.libros.find({titulo:{$regex: /es$/i}})
``` 
```json  
 db.libros.find({titulo:{$regex: 'es$', $options:'i'}})
``` 
 
 # Metodo sort (Ordenar Documentos)
 
 1. Ordenar los libros de manera accendente por el precio
```json  
 db.libros.find({},{titulo:1, precio:1, _id:0}).sort({precio:1})
``` 
 2. Ordenar los libros de manera descendente por el precio
```json  
 db.libros.find({},{titulo:1, precio:1, _id:0}).sort({precio:-1})
``` 
 3. Ordenar los libros de manera asencente por la editorial y de manera
 descendente por el precio, mostrando el titulo, el precio y la editorial
 ```json 
 db.libros.find({}, {titulo:1, precio:1, editorial:1, _id:0}).sort({editorial:1, precio:-1})
 ```
 # Otros Métodos skip, limit, size
 ```json 
 db.libros.find({}).size()
 db.libros.find({titulo:{$regex:/Java/i}}).size()
 ```
 -- Buscar todos los libros pero mostrando los dos primeros
```json  
 db.libros.find({},{titulo:1, editorial:1, precio:1, _id:0}).limit(2)
``` 
 -- Mostrar los 3 ultimos libros
```json  
 db.libros.find({},{titulo:1,editorial:1, precio:1, _id:0}).sort({precio:-1}).limit(3)
``` 
```json  
 db.getCollection('libros')
   .find({}, { titulo: 1, editorial: 1, _id: 0 })
   .sort({ titulo: 1, precio: -1 })
   .limit(4);
 ```
   # Borrar Colecciones y base de Datos
 ```json 
   use db5
   db.createCollection('ejemplo')
   show collections
 
   db.ejemplo.insertOne({nombre: 'Chapuin'})
   db.ejemplo.drop()
   db.dropDatabase()
  ``` 
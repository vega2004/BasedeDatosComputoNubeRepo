
# Consultas de Agregación en MongoDB

### 1. Contar los productos de tipo "medio":
```json
db.productos.countDocuments({ tipo: "medio" });
```

### Resultado: 
``` json
db1> db.productos.countDocuments({ tipo: "medio" });
25
```

### 2. Indicar las empresas (fabricantes) con un `distinct`:
```json
db.productos.distinct("fabricante");
```

### Resultado:
```json
[
  'A.O. Smith',
  'Alere',
  'American Tire Distributors Holdings',
  'Anthem',
  'Archrock',
  'Ascena Retail Group',
  'AutoNation',
  'Best Buy',
  'CIT Group',
  'Cabot',
  'Comcast',
  'Comerica',
  'Core-Mark Holding',
  'DST Systems',
  'Darling Ingredients',
  'Delta Air Lines',
  'Delta Tucker Holdings',
  "Dick's Sporting Goods",
  'First Solar',
  'HCA Holdings',
  'Hanesbrands',
  'Hartford Financial Services Group',
  'Hawaiian Holdings',
  'HealthSouth',
  'Hyatt Hotels',
  'Kar Auction Services',
  'Kelly Services',
  'Kemper',
  'Kimberly-Clark',
  'Lennar',
  'Mercury General',
  'Mondelez International',
  'Motorola Solutions',
  'Nasdaq OMX Group',
  'National Oilwell Varco',
  'Nordstrom',
  'OneMain Holdings',
  'Oneok',
  'Orbital ATK',
  'Pep Boys-Mann',
  'Pool',
  'Precision Castparts',
  'Primoris Services',
  'Raymond James Financial',
  'Seaboard',
  'Securian Financial Group',
  'Simon Property Group',
  'State Farm Insurance Cos.',
  'State Street Corp.',
  'SunPower',
  'TEGNA',
  'Telephone & Data Systems',
  'Total System Services',
  'Tractor Supply',
  'TransDigm Group',
  'Trinity Industries',
  'TrueBlue',
  'Universal American',
  'Universal Health Services',
  'WGL Holdings',
  "Wendy's",
  'Werner Enterprises',
  'WestRock',
  'Williams-Sonoma'
]
```

### 3. Visualizar los productos que tienen mas de 80 unidades utilizando `aggregate`:
```json
db.productos.aggregate([
  { $match: { unidades: { $gt: 80 } } }
]);
```

### Resultado:
```json
[
  {
    _id: ObjectId('67da3637f6ba2c26a91da19e'),
    codigo: 0,
    nombre: 'Fantastic Wooden Fish',
    unidades: 95,
    precio: 291,
    fabricante: 'Kimberly-Clark',
    tipo: 'avanzado'
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1a0'),
    codigo: 2,
    nombre: 'Small Soft Fish',
    unidades: 96,
    precio: 189,
    fabricante: 'Primoris Services',
    tipo: 'medio'
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1aa'),
    codigo: 12,
    nombre: 'Refined Concrete Salad',
    unidades: 90,
    precio: 129,
    fabricante: 'Universal Health Services',
    tipo: 'avanzado'
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1bc'),
    codigo: 30,
    nombre: 'Small Rubber Pants',
    unidades: 89,
    precio: 16,
    fabricante: 'Hanesbrands',
    tipo: 'basico'
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1bf'),
    codigo: 33,
    nombre: 'Generic Concrete Hat',
    unidades: 82,
    precio: 70,
    fabricante: 'American Tire Distributors Holdings',
    tipo: 'basico'
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1d3'),
    codigo: 53,
    nombre: 'Licensed Plastic Hat',
    unidades: 96,
    precio: 38,
    fabricante: 'Best Buy',
    tipo: 'medio'
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1d4'),
    codigo: 54,
    nombre: 'Generic Metal Sausages',
    unidades: 84,
    precio: 77,
    fabricante: 'DST Systems',
    tipo: 'medio'
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1db'),
    codigo: 61,
    nombre: 'Sleek Rubber Keyboard',
    unidades: 82,
    precio: 33,
    fabricante: 'Alere',
    tipo: 'basico'
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1e0'),
    codigo: 66,
    nombre: 'Incredible Concrete Fish',
    unidades: 96,
    precio: 336,
    fabricante: 'Darling Ingredients',
    tipo: 'medio'
  }
]
```

### 4. Usar `$project` para visualizar solo el nombre, unidades y precio de los productos con menos de 10 unidades:
```json
db.productos.aggregate([
  { $match: { unidades: { $lt: 10 } } },
  { $project: { _id:0, nombre: 1, unidades: 1, precio: 1 } }
]);
```

### Resultado:
```json
  { nombre: 'Ergonomic Metal Ball', unidades: 5, precio: 246 },
  { nombre: 'Handmade Plastic Hat', unidades: 7, precio: 253 },
  { nombre: 'Ergonomic Metal Table', unidades: 0, precio: 94 },
  { nombre: 'Practical Frozen Chips', unidades: 0, precio: 305 },
  { nombre: 'Fantastic Metal Pants', unidades: 5, precio: 129 },
  { nombre: 'Intelligent Frozen Sausages', unidades: 3, precio: 111 },
  { nombre: 'Rustic Plastic Mouse', unidades: 5, precio: 24 }
```

### 5. Cambiar el nombre del campo `fabricante` por "empresa" usando `$project`:
```json
db.productos.aggregate([
  { $match: { unidades: { $lt: 10 } } },
  { $project: { empresa: "$fabricante", nombre: 1, unidades: 1, precio: 1 } }
]);
```

### Resultado:
```json
[
  {
    _id: ObjectId('67da3637f6ba2c26a91da1a2'),
    nombre: 'Ergonomic Metal Ball',
    unidades: 5,
    precio: 246,
    empresa: 'Seaboard'
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1a8'),
    nombre: 'Handmade Plastic Hat',
    unidades: 7,
    precio: 253,
    empresa: "Dick's Sporting Goods"
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1b2'),
    nombre: 'Ergonomic Metal Table',
    unidades: 0,
    precio: 94,
    empresa: 'Kelly Services'
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1cd'),
    nombre: 'Practical Frozen Chips',
    unidades: 0,
    precio: 305,
    empresa: 'Delta Air Lines'
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1d1'),
    nombre: 'Fantastic Metal Pants',
    unidades: 5,
    precio: 129,
    empresa: 'OneMain Holdings'
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1d6'),
    nombre: 'Intelligent Frozen Sausages',
    unidades: 3,
    precio: 111,
    empresa: 'A.O. Smith'
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1dd'),
    nombre: 'Rustic Plastic Mouse',
    unidades: 5,
    precio: 24,
    empresa: 'Orbital ATK'
  }
]
```


### 6. Añadir un campo calculado llamado "total" que sea el precio por unidades:
```json
db.productos.aggregate([
  { $match: { unidades: { $lt: 10 } } },
  { $project: { empresa: "$fabricante", nombre: 1, unidades: 1, precio: 1, total: { $multiply: ["$precio", "$unidades"] } } }
]);
```

### Resultado:
```json
[
  {
    _id: ObjectId('67da3637f6ba2c26a91da1a2'),
    nombre: 'Ergonomic Metal Ball',
    unidades: 5,
    precio: 246,
    empresa: 'Seaboard',
    total: 1230
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1a8'),
    nombre: 'Handmade Plastic Hat',
    unidades: 7,
    precio: 253,
    empresa: "Dick's Sporting Goods",
    total: 1771
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1b2'),
    nombre: 'Ergonomic Metal Table',
    unidades: 0,
    precio: 94,
    empresa: 'Kelly Services',
    total: 0
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1cd'),
    nombre: 'Practical Frozen Chips',
    unidades: 0,
    precio: 305,
    empresa: 'Delta Air Lines',
    total: 0
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1d1'),
    nombre: 'Fantastic Metal Pants',
    unidades: 5,
    precio: 129,
    empresa: 'OneMain Holdings',
    total: 645
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1d6'),
    nombre: 'Intelligent Frozen Sausages',
    unidades: 3,
    precio: 111,
    empresa: 'A.O. Smith',
    total: 333
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1dd'),
    nombre: 'Rustic Plastic Mouse',
    unidades: 5,
    precio: 24,
    empresa: 'Orbital ATK',
    total: 120
  }
]
```

### 7. Hacer que el nombre salga en mayúsculas con el operador `$toUpper`:
```json
db.productos.aggregate([
  { $match: { unidades: { $lt: 10 } } },
  { $project: { empresa: "$fabricante", nombre: { $toUpper: "$nombre" }, unidades: 1, precio: 1, total: { $multiply: ["$precio", "$unidades"] } } }
]);
```

### Resultado:
```json
[
  {
    _id: ObjectId('67da3637f6ba2c26a91da1a2'),
    unidades: 5,
    precio: 246,
    empresa: 'Seaboard',
    nombre: 'ERGONOMIC METAL BALL',
    total: 1230
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1a8'),
    unidades: 7,
    precio: 253,
    empresa: "Dick's Sporting Goods",
    nombre: 'HANDMADE PLASTIC HAT',
    total: 1771
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1b2'),
    unidades: 0,
    precio: 94,
    empresa: 'Kelly Services',
    nombre: 'ERGONOMIC METAL TABLE',
    total: 0
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1cd'),
    unidades: 0,
    precio: 305,
    empresa: 'Delta Air Lines',
    nombre: 'PRACTICAL FROZEN CHIPS',
    total: 0
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1d1'),
    unidades: 5,
    precio: 129,
    empresa: 'OneMain Holdings',
    nombre: 'FANTASTIC METAL PANTS',
    total: 645
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1d6'),
    unidades: 3,
    precio: 111,
    empresa: 'A.O. Smith',
    nombre: 'INTELLIGENT FROZEN SAUSAGES',
    total: 333
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1dd'),
    unidades: 5,
    precio: 24,
    empresa: 'Orbital ATK',
    nombre: 'RUSTIC PLASTIC MOUSE',
    total: 120
  }
]
```

### 8. Añadir un campo calculado llamado "completo" que concatene el nombre del producto y el tipo:
```json
db.productos.aggregate([
  { $match: { unidades: { $lt: 10 } } },
  { $project: { empresa: "$fabricante", nombre: { $toUpper: "$nombre" }, unidades: 1, precio: 1, total: { $multiply: ["$precio", "$unidades"] }, completo: { $concat: ["$nombre", " ", "$tipo"] } } }
]);
```
### Resultado:
```json
[
  {
    _id: ObjectId('67da3637f6ba2c26a91da1a2'),
    unidades: 5,
    precio: 246,
    empresa: 'Seaboard',
    nombre: 'ERGONOMIC METAL BALL',
    total: 1230,
    completo: 'Ergonomic Metal Ball medio'
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1a8'),
    unidades: 7,
    precio: 253,
    empresa: "Dick's Sporting Goods",
    nombre: 'HANDMADE PLASTIC HAT',
    total: 1771,
    completo: 'Handmade Plastic Hat medio'
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1b2'),
    unidades: 0,
    precio: 94,
    empresa: 'Kelly Services',
    nombre: 'ERGONOMIC METAL TABLE',
    total: 0,
    completo: 'Ergonomic Metal Table avanzado'
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1cd'),
    unidades: 0,
    precio: 305,
    empresa: 'Delta Air Lines',
    nombre: 'PRACTICAL FROZEN CHIPS',
    total: 0,
    completo: 'Practical Frozen Chips medio'
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1d1'),
    unidades: 5,
    precio: 129,
    empresa: 'OneMain Holdings',
    nombre: 'FANTASTIC METAL PANTS',
    total: 645,
    completo: 'Fantastic Metal Pants basico'
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1d6'),
    unidades: 3,
    precio: 111,
    empresa: 'A.O. Smith',
    nombre: 'INTELLIGENT FROZEN SAUSAGES',
    total: 333,
    completo: 'Intelligent Frozen Sausages basico'
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1dd'),
    unidades: 5,
    precio: 24,
    empresa: 'Orbital ATK',
    nombre: 'RUSTIC PLASTIC MOUSE',
    total: 120,
    completo: 'Rustic Plastic Mouse avanzado'
  }
]
```

### 9. Ordenar el resultado por el campo "total":
```json
db.productos.aggregate([
  { $match: { unidades: { $lt: 10 } } },
  { $project: { empresa: "$fabricante", nombre: { $toUpper: "$nombre" }, unidades: 1, precio: 1, total: { $multiply: ["$precio", "$unidades"] }, completo: { $concat: ["$nombre", " ", "$tipo"] } } },
  { $sort: { total: 1 } }
]);
```
### Resultado:
```json
[
  {
    _id: ObjectId('67da3637f6ba2c26a91da1b2'),
    unidades: 0,
    precio: 94,
    empresa: 'Kelly Services',
    nombre: 'ERGONOMIC METAL TABLE',
    total: 0,
    completo: 'Ergonomic Metal Table avanzado'
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1cd'),
    unidades: 0,
    precio: 305,
    empresa: 'Delta Air Lines',
    nombre: 'PRACTICAL FROZEN CHIPS',
    total: 0,
    completo: 'Practical Frozen Chips medio'
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1dd'),
    unidades: 5,
    precio: 24,
    empresa: 'Orbital ATK',
    nombre: 'RUSTIC PLASTIC MOUSE',
    total: 120,
    completo: 'Rustic Plastic Mouse avanzado'
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1d6'),
    unidades: 3,
    precio: 111,
    empresa: 'A.O. Smith',
    nombre: 'INTELLIGENT FROZEN SAUSAGES',
    total: 333,
    completo: 'Intelligent Frozen Sausages basico'
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1d1'),
    unidades: 5,
    precio: 129,
    empresa: 'OneMain Holdings',
    nombre: 'FANTASTIC METAL PANTS',
    total: 645,
    completo: 'Fantastic Metal Pants basico'
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1a2'),
    unidades: 5,
    precio: 246,
    empresa: 'Seaboard',
    nombre: 'ERGONOMIC METAL BALL',
    total: 1230,
    completo: 'Ergonomic Metal Ball medio'
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1a8'),
    unidades: 7,
    precio: 253,
    empresa: "Dick's Sporting Goods",
    nombre: 'HANDMADE PLASTIC HAT',
    total: 1771,
    completo: 'Handmade Plastic Hat medio'
  }
]
```

### 10. Número de productos por tipo de producto:
```json
db.productos.aggregate([
  { $group: { _id: "$tipo", count: { $sum: 1 } } }
]);
```
### Resultado:
```json
[
  { _id: 'avanzado', count: 18 },
  { _id: 'medio', count: 25 },
  { _id: 'basico', count: 24 }
]
```

### 11. Valor mayor y menor de productos por tipo:
```json
db.productos.aggregate([
  { $group: { _id: "$tipo", max: { $max: "$precio" }, min: { $min: "$precio" } } }
]);
```
### Resultado:
```json
[
  { _id: 'basico', max: 285, min: 16 },
  { _id: 'avanzado', max: 331, min: 18 },
  { _id: 'medio', max: 337, min: 16 }
]
```

### 12. Total de unidades por cada tipo de producto:
```json
db.productos.aggregate([
  { $group: { _id: "$tipo", totalUnidades: { $sum: "$unidades" } } }
]);
```
### Resultado:
```json
[
  { _id: 'avanzado', totalUnidades: 773 },
  { _id: 'medio', totalUnidades: 1224 },
  { _id: 'basico', totalUnidades: 1067 }
]
```

### 13. Usar `$set` y `$substr` para visualizar todos los datos del producto "Small Metal Tuna" y los primeros 5 caracteres del nombre:
```json
db.productos.aggregate([
  { $match: { nombre: "Small Metal Tuna" } },
  { $set: { nombre_corto: { $substr: ["$nombre", 0, 5] } } }
]);
```
### Resultado:
```json
[
  {
    _id: ObjectId('67da3637f6ba2c26a91da1d5'),
    codigo: 55,
    nombre: 'Small Metal Tuna',
    unidades: 46,
    precio: 43,
    fabricante: 'Raymond James Financial',
    tipo: 'medio',
    nombre_corto: 'Small'
  }
]
```

### 14. Guardar el nombre del artículo y el total en una colección llamada `productos2`:
```json
db.productos.aggregate([
  { $project: { nombre: 1, total: { $multiply: ["$precio", "$unidades"] } } },
  { $out: "productos2" }
]);
```

### 15. Comprobar que se ha creado la colección `productos2`:
```json
show collections;
```
### Resultado:
```json
db1> show collections
cazas
ciudades
ganancias_libros  [view]
libros
personas
productos
productos2
```

### 16. Realizar un `find` para comprobar el resultado en `productos2`:
```json
db.productos2.find({});
```
### Resultado:
```json
db1> db.productos2.find({})
[
  {
    _id: ObjectId('67da3637f6ba2c26a91da19e'),
    nombre: 'Fantastic Wooden Fish',
    total: 27645
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da19f'),
    nombre: 'Rustic Concrete Pants',
    total: 18414
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1a0'),
    nombre: 'Small Soft Fish',
    total: 18144
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1a1'),
    nombre: 'Practical Soft Pants',
    total: 2747
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1a2'),
    nombre: 'Ergonomic Metal Ball',
    total: 1230
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1a3'),
    nombre: 'Small Steel Gloves',
    total: 1665
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1a4'),
    nombre: 'Ergonomic Wooden Shirt',
    total: 14994
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1a5'),
    nombre: 'Handmade Steel Chair',
    total: 5392
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1a6'),
    nombre: 'Handcrafted Soft Gloves',
    total: 4512
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1a7'),
    nombre: 'Fantastic Concrete Salad',
    total: 12985
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1a8'),
    nombre: 'Handmade Plastic Hat',
    total: 1771
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1a9'),
    nombre: 'Refined Wooden Tuna',
    total: 8480
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1aa'),
    nombre: 'Refined Concrete Salad',
    total: 11610
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1ab'),
    nombre: 'Unbranded Soft Fish',
    total: 9434
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1ac'),
    nombre: 'Small Concrete Fish',
    total: 5040
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1ad'),
    nombre: 'Refined Concrete Bike',
    total: 2700
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1ae'),
    nombre: 'Tasty Cotton Pants',
    total: 3536
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1af'),
    nombre: 'Incredible Granite Gloves',
    total: 20590
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1b0'),
    nombre: 'Practical Metal Mouse',
    total: 6650
  },
  {
    _id: ObjectId('67da3637f6ba2c26a91da1b1'),
    nombre: 'Handcrafted Steel Chicken',
    total: 6215
  }
]
```

### 17. Usar `$cond` y `$project` para visualizar el nombre del producto, el precio y un campo llamado valoracion (con "barato" si el precio es menor de 250 y "caro" si es mayor o igual):
```json
db.productos.aggregate([
  { $project: { _id:0, nombre: 1, precio: 1, valoracion: { $cond: { if: { $lt: ["$precio", 250] }, then: "barato", else: "caro" } } } }
]);
```

### Resultado:
```json
[
  { nombre: 'Fantastic Wooden Fish', precio: 291, valoracion: 'caro' },
  { nombre: 'Rustic Concrete Pants', precio: 279, valoracion: 'caro' },
  { nombre: 'Small Soft Fish', precio: 189, valoracion: 'barato' },
  { nombre: 'Practical Soft Pants', precio: 67, valoracion: 'barato' },
  { nombre: 'Ergonomic Metal Ball', precio: 246, valoracion: 'barato' },
  { nombre: 'Small Steel Gloves', precio: 37, valoracion: 'barato' },
  {
    nombre: 'Ergonomic Wooden Shirt',
    precio: 238,
    valoracion: 'barato'
  },
  { nombre: 'Handmade Steel Chair', precio: 337, valoracion: 'caro' },
  {
    nombre: 'Handcrafted Soft Gloves',
    precio: 282,
    valoracion: 'caro'
  },
  {
    nombre: 'Fantastic Concrete Salad',
    precio: 265,
    valoracion: 'caro'
  },
  { nombre: 'Handmade Plastic Hat', precio: 253, valoracion: 'caro' },
  { nombre: 'Refined Wooden Tuna', precio: 212, valoracion: 'barato' },
  {
    nombre: 'Refined Concrete Salad',
    precio: 129,
    valoracion: 'barato'
  },
  { nombre: 'Unbranded Soft Fish', precio: 178, valoracion: 'barato' },
  { nombre: 'Small Concrete Fish', precio: 126, valoracion: 'barato' },
  {
    nombre: 'Refined Concrete Bike',
    precio: 180,
    valoracion: 'barato'
  },
  { nombre: 'Tasty Cotton Pants', precio: 52, valoracion: 'barato' },
  {
    nombre: 'Incredible Granite Gloves',
    precio: 290,
    valoracion: 'caro'
  },
  {
    nombre: 'Practical Metal Mouse',
    precio: 190,
    valoracion: 'barato'
  },
  {
    nombre: 'Handcrafted Steel Chicken',
    precio: 113,
    valoracion: 'barato'
  }
]
```


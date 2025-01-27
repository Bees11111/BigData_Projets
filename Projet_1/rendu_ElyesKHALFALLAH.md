# Projet n°1 - Requetes sur MongoDB

Elyes KHALFALLAH - 5230635

---

---

## 2. Premières Requetes

### --- 2.1 -------------------------------------

**Requete** :

```javascript
db.grades.findOne();
```

```javascript
db.zips.findOne();
```

**Output** :

```javascript
{
  _id: ObjectId('50b59cd75bed76f46522c34e'),
  student_id: 0,
  class_id: 2,
  scores: [
    {
      type: 'exam',
      score: 57.92947112575566
    },
    {
      type: 'quiz',
      score: 21.24542588206755
    },
    {
      type: 'homework',
      score: 68.1956781058743
    },
    {
      type: 'homework',
      score: 67.95019716560351
    },
    {
      type: 'homework',
      score: 18.81037253352722
    }
  ]
}
```

Les objets de la collection grades comportent un id d'objet, un student_id, un class_id, et un tableau de scores comportant un exam, un quiz, et 0, 1, ou plusieurs homework, ainsi que leurs notes respectivement.

```javascript
{
  _id: '01001',
  city: 'AGAWAM',
  loc: [
    -72.622739,
    42.070206
  ],
  pop: 15338,
  state: 'MA'
}
```

Les objets de la collection zips comportent un id d'objet, un nom de ville, un tableau loc contenant 2 elements, representant certainement la latitute et longitude de la ville, puis sa population, et l'état (certainement américain) dans lequel la ville est.

### --- 2.2 -------------------------------------

**Requete** :

```javascript
db.grades.find({});
```

**Output** :

```javascript
{
  _id: ObjectId('50b59cd75bed76f46522c34e'),
  student_id: 0,
  class_id: 2,
  scores: [
    {
      type: 'exam',
      score: 57.92947112575566
    },
    {
      type: 'quiz',
      score: 21.24542588206755
    },
    {
      type: 'homework',
      score: 68.1956781058743
    },
    {
      type: 'homework',
      score: 67.95019716560351
    },
    {
      type: 'homework',
      score: 18.81037253352722
    }
  ]
}

etc...
```

### --- 2.3 -------------------------------------

**Requete** :

```javascript
db.grades.countDocuments();
```

**Output** :

```javascript
280;
```

### --- 2.4 -------------------------------------

**Requete** :

```javascript
db.grades.find({ class_id: 20 });
```

**Output** :

```javascript
{
  _id: ObjectId('50b59cd75bed76f46522c37a'),
  student_id: 6,
  class_id: 20,
  scores: [
    {
      type: 'exam',
      score: 55.41495979162525
    },
    {
      type: 'quiz',
      score: 80.78090944766167
    },
    {
      type: 'homework',
      score: 5.32775635719569
    }
  ]
}

etc...
```

Effectivement, nous obtenons 7 résultats avec cette requete.

### --- 2.5 -------------------------------------

**Requete** :

```javascript
db.grades.find({ class_id: { $lte: 20 } });
```

**Output** :

```javascript
{
  _id: ObjectId('50b59cd75bed76f46522c34e'),
  student_id: 0,
  class_id: 2,
  scores: [
    {
      type: 'exam',
      score: 57.92947112575566
    },
    {
      type: 'quiz',
      score: 21.24542588206755
    },
    {
      type: 'homework',
      score: 68.1956781058743
    },
    {
      type: 'homework',
      score: 67.95019716560351
    },
    {
      type: 'homework',
      score: 18.81037253352722
    }
  ]
}

etc...
```

### --- 2.6 -------------------------------------

**Requete** :

```javascript
db.grades.find({
  $expr: {
    $gte: ["$student_id", "$class_id"],
  },
});
```

**Output** :

```javascript
{
  _id: ObjectId('50b59cd75bed76f46522c366'),
  student_id: 3,
  class_id: 3,
  scores: [
    {
      type: 'exam',
      score: 86.2587791014086
    },
    {
      type: 'quiz',
      score: 0.7165465872248866
    },
    {
      type: 'homework',
      score: 70.07859698701473
    },
    {
      type: 'homework',
      score: 61.00984159293741
    }
  ]
}

etc...
```

### --- 2.7 -------------------------------------

**Requete** :

Première version avec un double filtre sur le class_id

```javascript
db.grades.find({
  class_id: {
    $lte: 20,
    $gte: 10,
  },
});
```

Deuxième version avec $and

```javascript
db.grades.find({
  $expr: {
    $and: [{ $gte: ["$class_id", 10] }, { $lte: ["$class_id", 20] }],
  },
});
```

**Output** :

```javascript
{
  _id: ObjectId('50b59cd75bed76f46522c357'),
  student_id: 0,
  class_id: 11,
  scores: [
    {
      type: 'exam',
      score: 58.83297411100884
    },
    {
      type: 'quiz',
      score: 49.66835710930263
    },
    {
      type: 'homework',
      score: 18.05861540807023
    },
    {
      type: 'homework',
      score: 80.04086698967356
    }
  ]
}

etc...
```

```javascript
{
  _id: ObjectId('50b59cd75bed76f46522c357'),
  student_id: 0,
  class_id: 11,
  scores: [
    {
      type: 'exam',
      score: 58.83297411100884
    },
    {
      type: 'quiz',
      score: 49.66835710930263
    },
    {
      type: 'homework',
      score: 18.05861540807023
    },
    {
      type: 'homework',
      score: 80.04086698967356
    }
  ]
}

etc...
```

Comme on peut le voir, les deux requetes donnent les meme résultats.

### --- 2.8 -------------------------------------

**Requete** :

```javascript
db.grades.find(
  {},
  {
    ue: "$class_id",
    etu: "$student_id",
    _id: 0,
  }
);
```

La condition `{}` signifie qu'on récupère tous les documents (sans condition). L'exclusion de `_id: 0` empeche d'inclure le champ `_id` dans le résultat, car il est ajouté par défaut sinon.

**Output** :

```javascript
{
  ue: 2,
  etu: 0
}
{
  ue: 5,
  etu: 0
}
{
  ue: 28,
  etu: 0
}

etc...
```

---

---

## 3. Prise en main de quelques étapes d'agrégation

### --- 3.1 -------------------------------------

**Requete** :

```javascript
db.grades.aggregate([
  {
    $project: {
      somme: { $add: ["$class_id", "$student_id"] },
      class_id: 1,
      student_id: 1,
    },
  },
]);
```

**Output** :

```javascript
{
  _id: ObjectId('50b59cd75bed76f46522c34e'),
  student_id: 0,
  class_id: 2,
  somme: 2
}
{
  _id: ObjectId('50b59cd75bed76f46522c350'),
  student_id: 0,
  class_id: 5,
  somme: 5
}

etc...
```

### --- 3.2 -------------------------------------

**Requete** :

```javascript
db.zips.aggregate([
  {
    $match: {
      pop: { $gte: 10000 },
    },
  },
]);
```

**Output** :

```javascript
{
  _id: '01001',
  city: 'AGAWAM',
  loc: [
    -72.622739,
    42.070206
  ],
  pop: 15338,
  state: 'MA'
}
{
  _id: '01002',
  city: 'CUSHMAN',
  loc: [
    -72.51565,
    42.377017
  ],
  pop: 36963,
  state: 'MA'
}

etc...
```

### --- 3.3 -------------------------------------

**Requete** :

```javascript
db.grades.aggregate([
  {
    $sort: {
      class_id: 1,
      student_id: 1,
    },
  },
]);
```

**Output** :

```javascript
{
  _id: ObjectId('50b59cd75bed76f46522c370'),
  student_id: 4,
  class_id: 0,
  scores: [
    {
      type: 'exam',
      score: 67.50593066420024
    },
    {
      type: 'quiz',
      score: 60.43052278095685
    },
    {
      type: 'homework',
      score: 98.634640715799
    },
    {
      type: 'homework',
      score: 6.719346717934882
    },
    {
      type: 'homework',
      score: 43.61408080940507
    },
    {
      type: 'homework',
      score: 92.01607786819618
    }
  ]
}
```

### --- 3.4 -------------------------------------

**Requete** :

```javascript
db.grades.aggregate([
  {
    $unwind: "$scores",
  },
]);
```

**Output** :

```javascript
{
  _id: ObjectId('50b59cd75bed76f46522c34e'),
  student_id: 0,
  class_id: 2,
  scores: {
    type: 'exam',
    score: 57.92947112575566
  }
}
```

### --- 3.5 -------------------------------------

**Requete** :

```javascript
db.zips.aggregate([
  {
    $group: {
      _id: "$city",
      minpop: { $min: "$pop" },
    },
  },
]);
```

**Output** :

```javascript
{
  _id: 'WILSONS',
  minpop: 592
}
{
  _id: 'MILES CITY',
  minpop: 10604
}
{
  _id: 'SAUK CITY',
  minpop: 5186
}

etc...
```

### --- 3.6 -------------------------------------

**Requete** :

```javascript
db.grades.aggregate([
  {
    $lookup: {
      from: "zips",
      localField: "student_id",
      foreignField: "pop",
      as: "zipsdocs",
    },
  },
]);
```

**Output** : Dernier output au lieu du premier, car la réponse à la requete est très longue

```javascript
{
  _id: ObjectId('50b59cd75bed76f46522c361'),
  student_id: 3,
  class_id: 10,
  scores: [
    {
      type: 'exam',
      score: 35.47946463550763
    },
    {
      type: 'quiz',
      score: 94.14222652833352
    },
    {
      type: 'homework',
      score: 58.43343860077279
    },
    {
      type: 'homework',
      score: 66.83562834109681
    }
  ],
  zipsdocs: [
    {
      _id: '04570',
      city: 'SQUIRREL ISLAND',
      loc: [
        -69.630974,
        43.809031
      ],
      pop: 3,
      state: 'ME'
    },
    {
      _id: '60604',
      city: 'CHICAGO',
      loc: [
        -87.632999,
        41.87845
      ],
      pop: 3,
      state: 'IL'
    },
    {
      _id: '99692',
      city: 'DUTCH HARBOR',
      loc: [
        -167.510656,
        53.362757
      ],
      pop: 3,
      state: 'AK'
    }
  ]
}
```

---

---

## 4. Pipelines d'agrégation

### --- 4.1 -------------------------------------

**Requete** :

```javascript
db.grades.aggregate([
  {
    $unwind: "$scores",
  },
  {
    $group: {
      _id: "$scores.type",
      nombre: { $sum: 1 },
    },
  },
]);
```

**Output** :

```javascript
{
  _id: 'homework',
  nombre: 681
}
{
  _id: 'quiz',
  nombre: 280
}
{
  _id: 'exam',
  nombre: 280
}
```

### --- 4.2 -------------------------------------

**Requete** :

```javascript
db.grades.aggregate([
  {
    $unwind: "$scores",
  },
  {
    $match: { "scores.type": "exam" },
  },
  {
    $group: {
      _id: "$class_id",
      bestGrade: { $max: "$scores.score" },
    },
  },
]);
```

**Output** :

```javascript
{
  _id: 16,
  bestGrade: 92.71381900055565
}
{
  _id: 2,
  bestGrade: 91.62924972289747
}
{
  _id: 18,
  bestGrade: 92.62628390055252
}

etc...
```

### --- 4.3 -------------------------------------

**Requete** :

```javascript
db.zips.aggregate([
  {
    $group: {
      _id: { city: "$city", state: "$state" },
      total: { $sum: "$pop" },
    },
  },
  {
    $sort: { total: -1 },
  },
  {
    $limit: 10,
  },
  {
    $project: {
      city: "$_id.city",
      state: "$_id.state",
      total: 1,
    },
  },
]);
```

**Output** :

```javascript
{
  _id: {
    city: 'CHICAGO',
    state: 'IL'
  },
  total: 2452177,
  city: 'CHICAGO',
  state: 'IL'
}
{
  _id: {
    city: 'BROOKLYN',
    state: 'NY'
  },
  total: 2300504,
  city: 'BROOKLYN',
  state: 'NY'
}

etc...
```

### --- 4.4 -------------------------------------

**Requete** :

```javascript
db.zips.aggregate([
  {
    $group: {
      _id: { city: "$city", state: "$state" },
      popCity: { $sum: "$pop" },
    },
  },
  {
    $group: {
      _id: "$_id.state",
      popAverage: { $avg: "$popCity" },
    },
  },
]);
```

**Output** :

```javascript
{
  _id: 'IN',
  popAverage: 9271.130434782608
}
{
  _id: 'TX',
  popAverage: 13775.02108678021
}
{
  _id: 'NJ',
  popAverage: 15775.89387755102
}

etc...
```

### --- 4.5 -------------------------------------

**Requete** :

```javascript
db.grades.aggregate([
  {
    $unwind: "$scores",
  },
  {
    $match: {
      "scores.type": "exam",
      "scores.score": { $gte: 50.0 },
    },
  },
  {
    $group: {
      _id: "$class_id",
      students: { $push: "$student_id" },
    },
  },
]);
```

**Output** :

```javascript
{
  _id: 16,
  students: [
    1,
    0,
    9,
    29,
    32,
    37,
    40
  ]
}
{
  _id: 30,
  students: [
    5,
    20
  ]
}

etc...
```

### --- 4.6 -------------------------------------

**Requete** :

```javascript
db.grades.aggregate([
  {
    $unwind: "$scores",
  },
  {
    $group: {
      _id: {
        class_id: "$class_id",
        student_id: "$student_id",
        type: "$scores.type",
      },
      mean: { $avg: "$scores.score" },
    },
  },
  {
    $group: {
      _id: {
        class_id: "$_id.class_id",
        student_id: "$_id.student_id",
      },
      gradeAverage: { $avg: "$mean" },
    },
  },
]);
```

**Output** :

```javascript
{
  _id: {
    class_id: 29,
    student_id: 30
  },
  gradeAverage: 70.03373378038405
}
{
  _id: {
    class_id: 16,
    student_id: 18
  },
  gradeAverage: 49.7360385324071
}

etc...
```

### --- 4.7 -------------------------------------

**Requete** :

```javascript
db.grades.aggregate([
  {
    $unwind: "$scores",
  },
  {
    $group: {
      _id: {
        class_id: "$class_id",
        type: "$scores.type",
      },
      moy: { $avg: "$scores.score" },
    },
  },
  {
    $group: {
      _id: "$_id.class_id",
      bestMean: { $max: "$moy" },
      typeMean: { $push: { type: "$_id.type", moy: "$moy" } },
    },
  },
  {
    $unwind: "$typeMean",
  },
  {
    $match: {
      $expr: {
        $eq: ["$bestMean", "$typeMean.moy"],
      },
    },
  },
  {
    $project: {
      class_id: "$_id",
      bestGradeType: "$typeMean.type",
    },
  },
]);
```

**Output** :

```javascript
{
  _id: 16,
  class_id: 16,
  bestGradeType: 'quiz'
}
{
  _id: 2,
  class_id: 2,
  bestGradeType: 'quiz'
}
{
  _id: 18,
  class_id: 18,
  bestGradeType: 'exam'
}

etc...
```

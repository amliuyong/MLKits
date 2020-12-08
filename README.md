# ML Kits

Starter projects for learning about Machine Learning.

## Downloading

There are two ways to download this repository - either as a zip or by using git.

### Zip Download

To download this project as a zip file, find the green 'Clone or Download' button on the top right hand side. Click that button, then download this project as a zip file.

Once downloaded extract the zip file to your local computer.

### Git Download

To download this project using git, run the following command at your terminal:

```
git clone https://github.com/StephenGrider/MLKits.git
```

# lodash & JSPlaygrounds

- https://lodash.com/
- https://stephengrider.github.io/JSPlaygrounds/

```javascript

const numbers = [
    [10, 5],
    [11, 2],
    [9, 16],
    [10, -1]
  ];

const sorted = _.sortBy(numbers, (row) => row[1]);

const mapped = _.map(sorted, row => row[1]);

_.chain(numbers)
.sortBy( (row) => row[1])
.map(row => row[1])
.value();


```

# KNN by lodash
```javascript

const outputs = [];
//import _ from "lodash";

function onScoreUpdate(dropPosition, bounciness, size, bucketLabel) {
  // Ran every time a balls drops into a bucket
  outputs.push([dropPosition, bounciness, size, bucketLabel]);
  //console.log(outputs);
}

function distance(pointA, pointB) {
  // pointA = [300, 0.5, 16], pointB = [120, 0.45, 16],
  return (
    _.chain(pointA)
      .zip(pointB)
      .map(([a, b]) => (a - b) ** 2)
      .sum()
      .value() ** 0.5
  );
}

function runAnalysis_0() {
  // Write code here to analyze stuff
  const testSetSize = 100;
  const [testSet, trainSet] = splitDataset(minMax(outputs, 3), testSetSize);

  // let numberCorrect = 0;
  // for(let i =0; i< testSet.length; i++) {
  //   const bucket =  knn(trainSet, testSet[i][0]);
  //   console.log(bucket, testSet[i][3]);

  //   if (bucket ===  testSet[i][3]) {
  //     numberCorrect++;
  //   }
  // }

  // console.log('Accuracy:', numberCorrect /testSetSize );

  _.range(1, 20).forEach((k) => {
    const accuracy = _.chain(testSet)
      .filter(
        (testPoint) =>
          knn(trainSet, _.initial(testPoint), k) === _.last(testPoint)
      )
      .size()
      .divide(testSetSize)
      .value();

    console.log(`k=${k}, Accuracy:${accuracy}`);
  });
}




function runAnalysis() {
  // Write code here to analyze stuff
  const testSetSize = 100;
  const k = 10;

  _.range(0, 3).forEach(feature => {

    const data = _.map(outputs, row => [row[feature], _.last(row)]);

    const [testSet, trainSet] = splitDataset(minMax(data, 1), testSetSize);

    const accuracy = _.chain(testSet)
      .filter(
        (testPoint) =>
          knn(trainSet, _.initial(testPoint), k) === _.last(testPoint)
      )
      .size()
      .divide(testSetSize)
      .value();

    console.log(`feature=${feature}, Accuracy:${accuracy}`);
  });
}


//point = [300, 0.5, 16]
//row = [300, 0.5, 16, 5]
function knn(dataset, point, k) {
  return _.chain(dataset)
    .map((row) => {
      return [distance(_.initial(row), point), _.last(row)];
    })
    .sortBy((row) => row[0])
    .slice(0, k)
    .countBy((row) => row[1])
    .toPairs()
    .sortBy((row) => row[1])
    .last()
    .first()
    .parseInt()
    .value();
}

function splitDataset(data, testCount) {
  const shuffed = _.shuffle(data);
  const testSet = _.slice(shuffed, 0, testCount);
  const trainSet = _.slice(shuffed, testCount);
  return [testSet, trainSet];
}

// Normalize data
function minMax(data, featureCount) {
  const clonedData = _.cloneDeep(data);

  for (let i = 0; i < featureCount; i++) {
    const cloumn = clonedData.map((row) => row[i]);
    const min = _.min(cloumn);
    const max = _.max(cloumn);

    for (let j = 0; j < clonedData.length; j++) {
      clonedData[j][i] = (clonedData[j][i] - min) / (max - min);
    }
  }

  return clonedData;
}


```
# TesnorFlow

## tensor
```javascript

const data = tf.tensor(
  [
  [1,2,3],
  [4,5,6]
  ]
);
const otherData = tf.tensor(
[
  [4,5,6],
  [6,7,8] 
]
);

data.add(otherData)

data.div(otherData)

data.sub(otherData)

// for debug
data.print()


// get data from tensor
const data = tf.tensor([10, 20, 30]);
data.get(0);

const data2D = tf.tensor([[10, 20, 30],
                          [3, 5, 9]]);

data2D.get(1, 1);

data2D.set(1, 1, 60); // NO SE T!!!!


```
## slice
```javascript

const data = tf.tensor([
   [10, 20, 30 ],
   [10, 21, 30 ],
   [10, 22, 30 ],
   [10, 23, 30 ],
   [10, 24, 30 ],
   [10, 25, 30 ],
   [10, 26, 30 ],
   [10, 27, 30 ]  
  ]);

// slice(startIndex, size) 

data.slice([0, 1], [8, 1]); 
// => [[20], [21], [22], [23], [24], [25], [26], [27]]

data.shape // [8,3]

data.slice([0, 1], [data.shape[0], 1]); 
// => [[20], [21], [22], [23], [24], [25], [26], [27]]

data.slice([1, 1], [-1, 1]); 
// => [[21], [22], [23], [24], [25], [26], [27]]

```

## concat

```javascript

const tensorA = tf.tensor([
  [1, 2, 3],
  [4, 5, 6]
]);

const tensorB = tf.tensor([
  [12, 22, 32],
  [42, 52, 62]
]);


tensorA.concat(tensorB)
// [[1 , 2 , 3 ], [4 , 5 , 6 ], [12, 22, 32], [42, 52, 62]]

tensorA.concat(tensorB, 0)
// [[1 , 2 , 3 ], [4 , 5 , 6 ], [12, 22, 32], [42, 52, 62]]


tensorA.concat(tensorB, 1)
// [[1, 2, 3, 12, 22, 32], [4, 5, 6, 42, 52, 62]]

```

## sum, expandDims, concat
```
const jumpData = tf.tensor([
   [70, 70, 70],
   [70, 70, 70],
   [70, 70, 70],
   [70, 70, 70],

]);

const playerData = tf.tensor([
  [1, 160],
  [2, 160],
  [3, 160],
  [4, 160],
]);


// what I want: 
[
 [210, 1, 160], 
 [210, 2, 160], 
 [210, 3, 160], 
 [210, 4, 160]
 ]


// Method 1
jumpData.sum(1);
//=> [210, 210, 210, 210]
jumpData.sum(1, true)
// => [[210], [210], [210], [210]]
playerData.shape // [4,2]
jumpData.sum(1, true).concat(playerData, 1);
// => [[210, 1, 160], [210, 2, 160], [210, 3, 160], [210, 4, 160]]

// Mothed 2
jumpData.sum(1); // [210, 210, 210, 210]
jumpData.sum(1).expandDims() //[[210, 210, 210, 210],]
jumpData.sum(1).expandDims(1) // [[210], [210], [210], [210]]
jumpData.sum(1).expandDims(1).concat(playerData, 1);

```

## Broadcasting

Can do Broadcasting

- shape[3] + shape[1]  

- shape[2, 3] + shape[2, 1]

- shape[2,3,2] + shape[3, 1]

Canot do Broadcasting

 - shape[2,3,2] + shape[2, 1]





# Javascript Statistics Snippets

<img width="410" alt="Untitled" src="https://user-images.githubusercontent.com/6764693/57027780-e937e680-6c02-11e9-832a-cb42ff6de5dd.png">

Ever wanted to do something simple like generate random values from a distribution but didn't want to import a whole new library? 
This repo contains an ever growing list of the functions I have written for doing things related to statistics. Such as generating random values from distributions etc. 

## Distributions

__Exponential__

```js
function gen_expon(lambda){
  return -Math.log(1-Math.random())/lambda;
}
```

__Poisson__
```js
function gen_pois(lambda, max_its = 1000){  
  let i = -1, cum_sum = 0;
  while(cum_sum < 1 && i < max_its){
    i++;
    cum_sum += -Math.log(1-Math.random())/lambda;  // Or use gen_expon()
  }
  return i;
}
```

__Discrete Uniform__

```js
function gen_discrete_unif(min = 0, max = 100){
  const range = max - min;
  
  return Math.round(Math.random()*range) + min;
}
```



## Array helpers

---

### Copy array

Do a psuedo deep copy on an array of objects.

```js
function copy_array(arr){
  return arr.map(obj => Object.assign({}, obj));
}

copy_array([{a: 1, b: 2}, {a: 2, b: 5}]);
//Array(2) [
//  0: Object {a: 1, b: 2}
//  1: Object {a: 2, b: 5}
//]
```

---
### Array product
Get product of all elements in an array. 


```js
function product(arr){
  return arr.reduce((p, d) => p*d, 1);
}

product([1,2,3])
// 6
```

---
### Sum of elements

Sums everything in the array into single number.

```js
function sum(arr){
  return arr.reduce((s, el) => s + el, 0);
}

sum([1,2,3,4])
//10
```

---
### Normalize 

Normalize each element of the array such that the sum of all elements in the array is 1.


```js
function normalize(arr){
  const total_size = arr.reduce((s, el) => s + el, 0); 
  // or sum(arr);
  return arr.map(el => el/total_size);
}

normalize([1,2,3,4]);
//Array(4) [0.1, 0.2, 0.3, 0.4]

sum(normalize([2,3,4,6,7]));
// 1
```

---
__Weighted Sample__

Draw a single element from an array with probability according to a weight array. 

```js
function weighted_sample(values, weights){
  const random_val = Math.random();
  let cumulative_prob = 0, i;
  for(i = 0; i < values.length; i++){
    cumulative_prob += weights[i];
    if(cumulative_prob > random_val) break;
  }
  return values[i];
}

weighted_sample(['a', 'b', 'c'], [0.1, 0.6, 0.3]);
//"b"
```

---
### Flatten (1-level) nested arrays 

Turns an array of arrays into a single array. Note: only works with one level of nesting. 

```js
function flatten(arr){
  return [].concat(...arr);
}

flatten([
  ['a', 'b'], 
  ['c', 'd'], 
  ['e', 'f']
]);
//Array(6) ['a', 'b', 'c', 'd', 'e', 'f']
```

__Initialize array of given size__

Generate a new array of a given size. Fill with either the index, nothing, a constant, or a function. 

```js
function init_array(n, fill = (_,i) => i){  
  const empty_array = [...new Array(n)];
  const fill_is_func = typeof fill === 'function';

  return fill === null ?
    empty_array :
    empty_array.map(fill_is_func ? fill : () => fill);
}

// Defaults to filling with index
init_array(5);
//Array(5) [0, 1, 2, 3, 4]

// Can simply supply empty array
init_array(5, null);
//Array(5) [undefined, undefined, undefined, undefined, undefined]

// Fill with constant
init_array(5, 'a');
//Array(5) ["a", "a", "a", "a", "a"]

// Combine with probability functions
init_array(5, () => gen_pois(10));
// Array(5) [11, 12, 6, 6, 8]
```

__Get unique elements of array__

```js
function unique(vec){
  return [...new Set(vec)];
}

unique(['a', 'b', 'a', 'c', 'c', 'd']);
//Array(4) ["a", "b", "c", "d"]
```

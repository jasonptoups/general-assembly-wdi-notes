## Selection Sort
1. Loop through the array to find the lowest number
2. Move the lowest number to the front 
3. Do this N times
__Big-O__: N^2

## Bubble sort
1. Go through the array and compare two adjacent numbers. Swap to move the larger one to the back of the pair
2. 
__Big-O__: N^2, but it is good if your array is close to already sorted

```js
Array.prototype.swap = function (a, b) {
  let temp = this[a]
  this[a] = this[b]
  this[b] = temp
}
// Here we are defining a function to swap two items in an array

function bubbleSort (arr) {
  let keepGoing = true
  while (keepGoing === true) {
    keepGoing = false
    // if the for loop doesn't meet the true, this will remain false, so the while loop will stop
    for (let i = 0; i < arr.length; i++) {
      if (arr[i] > arr[i+1]) {
        arr.swap(i, i+1)
        keepGoing = true
        // if arr[i] is greater than arr [i+1] then it swaps and marks the item true to keep going through the loop
      }
    }
  }
}
```

## Quick Sort
Quick sort creates a pivot value and sorts all the items in the array as above or below that pivot. Then it does the same thing through each of the sub arrays  
__Big-O__: technically N^2 but getting lower
```js
function quickSort (arr) {
  if (arr.length <= 1) {
    let below = []
    let above = []
    let pivot = arr.length - 1
    let pivotValue = arr[pivot]
    arr = arr.slice(0, pivot)
    for (let i = 0; i < a.length; i ++) {
		  a[i] < pivotValue ? above.push(a[i]) : below.push(a[i])
	}
    return quickSort(above).concat([pivotValue], quickSort(below))
  }
}
```

## Merge Sort
__Big-O__: N*log N  
Creates subarrays, sorts them, and then sorts two subarrays together. I guess this works somehow because it's easy to sort two things?  
Splits the array in half. Then sorts if it can. If not, split down the middle again and try to sort. 


# Insertion Sort Algorithm

An insert sort algorithm is comparison type sorting algorithm that is used on arrays or lists. It works from left to right, comparing your current value to the previous (left) one over. It has been best described as sorting a hand of cards. You pick up a card (the current) and put it in your hand. Pick up another and if smaller than what's in your hand, you place it to the left of that. Pick up another card and again compare it to the right most card of your already sorted hand. If smaller, check the next one over, if not, just place it on the far right end. In the end, you have a sorted hand of cards.

## The Benefits

The insertion sort runs in quadratic time, which if you're familiar with Big O notation, is O (n ^2^ ) and not amazing. This true in both its worst and average run times. However, it can outperform more advanced algorithms when the list is smaller or already sorted to some degree. Also, because the insertion sort sorts the array or list in place, the space required to use it is only O(1) aka constant space. 

## The Code

To help explain, I'll be using card references from the example above. We'll start the code off with the current  being at index 1 (in the card sorting comparison, this is when we draw our second card). We start here (at two cards) because having one card means it is already sorted. And we will run this algorithm for the length of the array so we sort the entire list.

```javascript
for(let i = 1; i < arr.length; i++){

}
```

Now lets declare our current card and the comparison (the one to the left or previous):

```javascript
for(let i = 1; i < arr.length; i++){
	let curr = arr[i];
	let prev = i - 1;
}
```

 With our current and previous card set, we want to continue to compare the current to the previous (card to the left) *while* our current is 0 or larger (meaning not at the beginning of our array) and while the card to the left is larger than the current:

```javascript
for(let i = 1; i < arr.length; i++){
	let curr = arr[i];
	let prev = i - 1;
	while(curr >= 0 && arr[prev] > curr){
		
	}
}
```

Now for the actual moving of cards. If prev card is bigger than the curr card, prev card gets shifted  to the right ( if you console logged the array at this point, we would temporarily have a duplicate in our list). We then re-define our  previous as one more over left and repeat the while loop while our conditions are met.

```javascript
for(let i = 1; i < arr.length; i++){
	let curr = arr[i];
	let prev = i - 1;
	while(curr >= 0 && arr[prev] > curr){
		arr[prev + 1] = arr[prev];
    prev = prev - 1;
	}
}
```



Once the while loop conditions are no longer met, (our current is larger than the previous or previous is less than 0)we can then place the current in its final resting place

```javascript
for(let i = 1; i < arr.length; i++){
	let curr = arr[i];
	let prev = i - 1;
	while(curr >= 0 && arr[prev] > curr){
		arr[prev + 1] = arr[prev];
    prev --;
	}
	arr[prev + 1] = curr;
}

```

Now all we have to do is throw it in a function and we have a useable insert sort algorithm!

```javascript
const insertSort = (arr) => {
  for(let i = 1; i < arr.length; i++){
    let curr = arr[i];
    let prev = i - 1;
    while(curr >= 0 && arr[prev] > curr){
      //shifting of the cards
      arr[prev + 1] = arr[prev];
      //moving our comparison left one
      prev = prev - 1;
    }
    arr[prev + 1] = curr;
  }
	return arr;
}
```



## When To Use An Insertion Sort

-  When the array is mostly sorted. Because insertion sort moves from left to right, it does not need to move anything that is sorted (aka does not need to enter the while loop). Going into the while loop is what gives it a quadratic run time, so the less time not in it, the better it performs. 
- When we have memory constraints. As stated earlier, it sorts an array in place giving it a constant runtime in terms of memory. This may be the factor of why you would choose this over a better performing algorithm.
- When working with relatively small arrays (roughly less than 50 elements).
- When working with elements that are "online" or coming in so that they can be sorted as they arrive. 

## When Not To Use An Insertion Sort

- When the array is completely unsorted. 
- When the array has a large number of elements in it
- If memory is not an issue and you want faster results



## References

- https://www.acodersjourney.com/insertion-sort/
- https://en.wikipedia.org/wiki/Insertion_sort
- https://www.geeksforgeeks.org/insertion-sort/
- https://medium.com/javascript-algorithms/javascript-algorithms-insertion-sort-59b6b655373c
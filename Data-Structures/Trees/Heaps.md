# Questions Regarding Heaps
Find the Kth smallest/largest element in an array

```python
def find_k_smallest(arr):
    """
    Approach 1: sort the array O(NlogN)
                            if arranged in ascending order => (N-K+1) is the index
                            if arranged in descending order => K is the index

    Approach 2: Using a max heap where you pop k times => O(klogN)
                            max heap => root is always the maximum element
                                build the heap from array O(N) then pop it K times O(KlogN) 
                                => heapify is O(logN) so overall O(N + KlogN)

                            min heap => root is always the min element
                                build the heap from array O(N) then pop it (N-K+1) times O((N-K+1)log(N-K+1)) 
                                => heapify is O(logN) so overall O(N + (N-K+1)log(N-K+1))
                            if N-K+1 < K => use a min heap
                            for example if we have 100 elements and we need 97th largest elt:
                                max heap => pop 97 times
                                min heap => pop 4 times 

    Approach 3: Quick Select

    maintain a left and right pointer and select a pivot element => last elt
    keep track of the tail of the items that are less than the pivot => fix it as left
    and have an iteration variable right that checks the elements

    if the item on the right > pivot => advance the right pointer
    if the item on the right <= pivot => swap with the left pointer (tail of items less)
    finally swap the pivot with the item at tail => this item would be in its right position
    check if this position is (k-1) => found our kth elt
    if > (k-1) => elt present before pivot => shrink
    if < (k-1) => elt present after pivot => shrink
    [4, 2, 1, 5, 6, 3] => the pivot is 3 and left and right at 4 and 3

    """
def quick_select(arr, k):
    """
    searches for a single element
    find kth largest/smallest element in an array

    kth largest is (n - k + 1) smallest

    if the array is sorted in ascending order -> element at index N - k + 1
    if the array is sorted in descending order -> element at index k

    1. using merge sort -> O(nlogn)

    2. using max heap -> O(n + klogn) (building the heap O(n) then for each pop (k) we heapify O(logn))
    using a min heap -> O(n + (n-k+1)log(n-k+1)) (building the heap O(n) then for each pop (n-k) we heapify O(logn))
    if (n-k+1) < k => better to use a min heap

    3. using quick select -> in worst time it is O(n^2) but it rarely occurs -> [1, 2, 3, 4, 5] and k = 1
    
    best case is O(n)
    """
    left, right = 0, len(arr) - 1
    while left <= right:
        # find the right position of the pivot (last element)
        pivot_index = partition(arr, left, right)
        # if the pivot index is the kth elt -> return it
        if pivot_index == k - 1:
            return arr[pivot_index]
        # if there are more than k - 1 elements to the left of pivot -> partition left side
        elif pivot_index > k - 1:
            right = pivot_index - 1
        else:
            left = pivot_index + 1

    return -1

def partition(arr, left, right):
    pivot_element = arr[right]
    left_pointer = left - 1
    for right_pointer in range(left, right):
        if arr[right_pointer] <= pivot_element:
            left_pointer += 1
            arr[left_pointer], arr[right_pointer] = (
                arr[right_pointer],
                arr[left_pointer],
            )

    arr[left_pointer + 1], arr[right] = arr[right], arr[left_pointer + 1]

    return left_pointer + 1
```

- Find K closest points to origin

```python
def k_closest_points_to_origin(points, k):
    """
    Given a list of points of the form [x, y], find the k closest points to origin
    based on Euclidean distance : sqrt(x^2 + y^2)
    Approach:
    1. get the distances, store in form of [distance, x, y]
    2. build min heap of distances
    3. pop k times
    time complexity -> O(N + klogN)
    """
    heap = []
    for x, y in points:
        distance = pow(x, 2) + pow(y, 2)
        heap.append([distance, x, y])
    heapq.heapify(heap)

    result = []
    while k > 0:
        distance, x, y = heapq.heappop(heap)
        result.append([x, y])
        k -= 1
    return result
```
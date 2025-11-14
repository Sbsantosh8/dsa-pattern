# Two Pointers Approach - Complete Beginner's Guide

## 🎯 What is the Two Pointers Approach?

The Two Pointers technique is a **simple and efficient way to solve array/string problems** by using two variables (pointers) to traverse through the data structure.

Think of it like this: Instead of checking every possible combination (which is slow), we use two "fingers" pointing at different positions and move them smartly to find our answer.

---

## 🤔 Why Use Two Pointers?

### Without Two Pointers (Brute Force):
```python
# Finding if sum of two numbers equals target
# Time: O(n²) - SLOW!
for i in range(len(arr)):
    for j in range(i+1, len(arr)):
        if arr[i] + arr[j] == target:
            return True
```

### With Two Pointers:
```python
# Same problem but MUCH faster
# Time: O(n) - FAST!
left, right = 0, len(arr) - 1
while left < right:
    current_sum = arr[left] + arr[right]
    if current_sum == target:
        return True
    elif current_sum < target:
        left += 1
    else:
        right -= 1
```

**Result: We reduced time from O(n²) to O(n)!** 🚀

---

## 📚 Types of Two Pointers Patterns

There are **3 main patterns** you need to know:

### 1️⃣ **Opposite Direction (Left & Right)**
- Start from both ends
- Move towards center
- **When to use:** Sorted arrays, palindromes, pair problems

### 2️⃣ **Same Direction (Slow & Fast)**
- Both start from beginning
- Move at different speeds
- **When to use:** Remove duplicates, sliding window, partitioning

### 3️⃣ **Sliding Window**
- Two pointers define a window
- Expand/shrink the window
- **When to use:** Subarray problems, continuous sequences

---

## 🎓 Pattern 1: Opposite Direction (Left & Right)

### Visual Example:
```
Array: [1, 3, 5, 7, 9, 11]
        ↑              ↑
      left          right

Move pointers based on condition:
- If sum too small → move left pointer right
- If sum too large → move right pointer left
```

### Problem 1: Two Sum (Sorted Array)
**Problem:** Find two numbers that add up to a target.

```python
def two_sum(arr, target):
    """
    arr = [1, 2, 3, 4, 5, 6]
    target = 9
    Answer: [3, 6] (indices 2 and 5)
    """
    left = 0
    right = len(arr) - 1
    
    while left < right:
        current_sum = arr[left] + arr[right]
        
        if current_sum == target:
            return [left, right]  # Found it!
        
        elif current_sum < target:
            # Sum is too small, need bigger number
            left += 1  # Move left pointer right
        
        else:  # current_sum > target
            # Sum is too large, need smaller number
            right -= 1  # Move right pointer left
    
    return None  # No pair found

# Test
print(two_sum([1, 2, 3, 4, 5, 6], 9))  # Output: [2, 5]
```

**Step-by-step trace:**
```
Target = 9
[1, 2, 3, 4, 5, 6]
 ↑              ↑
left          right
1 + 6 = 7 < 9 → move left

[1, 2, 3, 4, 5, 6]
    ↑           ↑
   left       right
2 + 6 = 8 < 9 → move left

[1, 2, 3, 4, 5, 6]
       ↑        ↑
      left    right
3 + 6 = 9 ✓ → Found!
```

### Problem 2: Valid Palindrome
**Problem:** Check if a string reads the same forwards and backwards.

```python
def is_palindrome(s):
    """
    s = "racecar"
    Answer: True
    
    s = "hello"
    Answer: False
    """
    # Remove non-alphanumeric and convert to lowercase
    s = ''.join(c.lower() for c in s if c.isalnum())
    
    left = 0
    right = len(s) - 1
    
    while left < right:
        if s[left] != s[right]:
            return False  # Characters don't match
        
        left += 1   # Move both pointers
        right -= 1  # towards center
    
    return True  # All characters matched!

# Test
print(is_palindrome("A man, a plan, a canal: Panama"))  # True
print(is_palindrome("race a car"))  # False
```

**Visual trace:**
```
"racecar"
 ↑     ↑
 r = r ✓ → continue

"racecar"
  ↑   ↑
  a = a ✓ → continue

"racecar"
   ↑ ↑
   c = c ✓ → continue

"racecar"
    ↑
  left >= right → STOP, it's palindrome!
```

### Problem 3: Container With Most Water
**Problem:** Find two lines that together with x-axis form a container that holds the most water.

```python
def max_area(height):
    """
    height = [1, 8, 6, 2, 5, 4, 8, 3, 7]
    Answer: 49 (between index 1 and 8)
    """
    max_water = 0
    left = 0
    right = len(height) - 1
    
    while left < right:
        # Calculate current area
        width = right - left
        current_height = min(height[left], height[right])
        current_area = width * current_height
        
        max_water = max(max_water, current_area)
        
        # Move the pointer with smaller height
        if height[left] < height[right]:
            left += 1
        else:
            right -= 1
    
    return max_water

# Test
print(max_area([1, 8, 6, 2, 5, 4, 8, 3, 7]))  # Output: 49
```

---

## 🎓 Pattern 2: Same Direction (Slow & Fast)

### Visual Example:
```
Array: [1, 1, 2, 2, 3, 4, 4, 5]
        ↑
      slow
        ↑
       fast

Fast pointer explores ahead
Slow pointer tracks position for unique elements
```

### Problem 4: Remove Duplicates from Sorted Array
**Problem:** Remove duplicates in-place, return length of unique elements.

```python
def remove_duplicates(arr):
    """
    arr = [1, 1, 2, 2, 3, 4, 4, 5]
    After: [1, 2, 3, 4, 5, ?, ?, ?]
    Return: 5 (length of unique elements)
    """
    if not arr:
        return 0
    
    slow = 0  # Tracks position for next unique element
    
    for fast in range(1, len(arr)):
        # Found a new unique element
        if arr[fast] != arr[slow]:
            slow += 1
            arr[slow] = arr[fast]
    
    return slow + 1  # Length of unique elements

# Test
arr = [1, 1, 2, 2, 3, 4, 4, 5]
length = remove_duplicates(arr)
print(arr[:length])  # [1, 2, 3, 4, 5]
```

**Step-by-step trace:**
```
[1, 1, 2, 2, 3, 4, 4, 5]
 s
 f
arr[0] = arr[1]? Yes → skip

[1, 1, 2, 2, 3, 4, 4, 5]
 s
    f
arr[0] ≠ arr[2]? Yes → slow++, copy

[1, 2, 2, 2, 3, 4, 4, 5]
    s
    f

Continue until fast reaches end...
Final: [1, 2, 3, 4, 5, 4, 4, 5]
          ↑
        slow = 4 → return 5
```

### Problem 5: Move Zeros to End
**Problem:** Move all zeros to the end while maintaining order of other elements.

```python
def move_zeros(arr):
    """
    arr = [0, 1, 0, 3, 12]
    After: [1, 3, 12, 0, 0]
    """
    slow = 0  # Position for next non-zero element
    
    # Move all non-zero elements to front
    for fast in range(len(arr)):
        if arr[fast] != 0:
            arr[slow] = arr[fast]
            slow += 1
    
    # Fill remaining positions with zeros
    while slow < len(arr):
        arr[slow] = 0
        slow += 1
    
    return arr

# Test
print(move_zeros([0, 1, 0, 3, 12]))  # [1, 3, 12, 0, 0]
```

### Problem 6: Partition Array (Even-Odd)
**Problem:** Move all even numbers to the left and odd numbers to the right.

```python
def partition_even_odd(arr):
    """
    arr = [3, 1, 2, 4]
    After: [2, 4, 3, 1] (or any arrangement with evens first)
    """
    slow = 0  # Position for next even number
    
    for fast in range(len(arr)):
        if arr[fast] % 2 == 0:  # Found even number
            # Swap with slow position
            arr[slow], arr[fast] = arr[fast], arr[slow]
            slow += 1
    
    return arr

# Test
print(partition_even_odd([3, 1, 2, 4]))  # [2, 4, 3, 1]
```

---

## 🎓 Pattern 3: Sliding Window

### Visual Example:
```
Array: [1, 3, 2, 6, -1, 4, 1, 8, 2]
Find max sum of 3 consecutive elements

Window:
[1, 3, 2] 6, -1, 4, 1, 8, 2  → sum = 6
 ↑     ↑
left  right

Slide right:
1, [3, 2, 6] -1, 4, 1, 8, 2  → sum = 11
    ↑     ↑
   left  right
```

### Problem 7: Maximum Sum Subarray of Size K
**Problem:** Find maximum sum of K consecutive elements.

```python
def max_sum_subarray(arr, k):
    """
    arr = [2, 1, 5, 1, 3, 2]
    k = 3
    Answer: 9 (subarray [5, 1, 3])
    """
    if len(arr) < k:
        return None
    
    # Calculate sum of first window
    window_sum = sum(arr[:k])
    max_sum = window_sum
    
    # Slide the window
    for i in range(k, len(arr)):
        # Remove leftmost element, add rightmost element
        window_sum = window_sum - arr[i - k] + arr[i]
        max_sum = max(max_sum, window_sum)
    
    return max_sum

# Test
print(max_sum_subarray([2, 1, 5, 1, 3, 2], 3))  # Output: 9
```

**Visual trace:**
```
k = 3
[2, 1, 5, 1, 3, 2]
[2, 1, 5] → sum = 8

Remove 2, Add 1:
[2, 1, 5, 1] → [1, 5, 1] → sum = 7

Remove 1, Add 3:
[1, 5, 1, 3] → [5, 1, 3] → sum = 9 (max!)

Remove 5, Add 2:
[5, 1, 3, 2] → [1, 3, 2] → sum = 6
```

### Problem 8: Longest Substring Without Repeating Characters
**Problem:** Find length of longest substring without duplicate characters.

```python
def longest_unique_substring(s):
    """
    s = "abcabcbb"
    Answer: 3 ("abc")
    
    s = "bbbbb"
    Answer: 1 ("b")
    """
    char_set = set()  # Track characters in current window
    left = 0
    max_length = 0
    
    for right in range(len(s)):
        # If duplicate found, shrink window from left
        while s[right] in char_set:
            char_set.remove(s[left])
            left += 1
        
        # Add current character to window
        char_set.add(s[right])
        
        # Update max length
        max_length = max(max_length, right - left + 1)
    
    return max_length

# Test
print(longest_unique_substring("abcabcbb"))  # 3
print(longest_unique_substring("bbbbb"))     # 1
```

**Visual trace:**
```
s = "abcabcbb"

Window: "a" → length = 1
        ↑
       l,r

Window: "ab" → length = 2
        ↑ ↑
        l r

Window: "abc" → length = 3
        ↑  ↑
        l  r

Window: "abc|a" → duplicate!
        ↑   ↑
        l   r
Remove 'a', move left

Window: "bca" → length = 3
         ↑  ↑
         l  r

Continue...
```

---

## 🎯 How to Identify When to Use Two Pointers

### Use **Opposite Direction** when:
✅ Array/string is **sorted**  
✅ Looking for **pairs** that satisfy a condition  
✅ Checking for **palindromes**  
✅ Need to **compare elements from both ends**

**Keywords:** "two sum", "pair", "palindrome", "sorted array"

### Use **Same Direction** when:
✅ Need to **remove/modify elements in-place**  
✅ **Partitioning** array based on condition  
✅ One pointer **explores**, other **tracks position**  
✅ "Slow and fast" pointer problems

**Keywords:** "remove duplicates", "move elements", "partition", "in-place"

### Use **Sliding Window** when:
✅ Looking for **continuous subarray/substring**  
✅ Fixed or variable **window size**  
✅ Need to find **maximum/minimum** in subarrays  
✅ **Optimization** problems with contiguous elements

**Keywords:** "subarray", "substring", "consecutive", "window", "continuous"

---

## 💡 Common Patterns & Templates

### Template 1: Opposite Direction
```python
def opposite_direction_template(arr):
    left = 0
    right = len(arr) - 1
    
    while left < right:
        # Process current state
        
        if condition_met:
            return result
        elif need_larger_value:
            left += 1
        else:
            right -= 1
    
    return default_result
```

### Template 2: Same Direction
```python
def same_direction_template(arr):
    slow = 0
    
    for fast in range(len(arr)):
        if arr[fast] meets_condition:
            # Do something with arr[slow]
            arr[slow] = arr[fast]
            slow += 1
    
    return slow  # or modified array
```

### Template 3: Sliding Window (Fixed Size)
```python
def sliding_window_fixed(arr, k):
    # Initialize first window
    window_sum = sum(arr[:k])
    result = window_sum
    
    # Slide window
    for i in range(k, len(arr)):
        window_sum = window_sum - arr[i-k] + arr[i]
        result = update(result, window_sum)
    
    return result
```

### Template 4: Sliding Window (Variable Size)
```python
def sliding_window_variable(arr):
    left = 0
    result = 0
    
    for right in range(len(arr)):
        # Expand window
        # Add arr[right] to window
        
        while window_invalid:
            # Shrink window from left
            # Remove arr[left] from window
            left += 1
        
        # Update result with current valid window
        result = max(result, right - left + 1)
    
    return result
```

---

## 🏋️ Practice Problems (Easy to Hard)

### Easy Level:
1. ✅ **Reverse String** - Use opposite direction
2. ✅ **Remove Element** - Use same direction
3. ✅ **Merge Sorted Array** - Use opposite direction
4. ✅ **Valid Palindrome** - Use opposite direction
5. ✅ **Remove Duplicates** - Use same direction

### Medium Level:
6. ⭐ **3Sum** - Multiple two pointers
7. ⭐ **Container With Most Water** - Opposite direction
8. ⭐ **Longest Substring Without Repeating** - Sliding window
9. ⭐ **Minimum Window Substring** - Sliding window
10. ⭐ **Sort Colors** - Same direction (Dutch flag)

### Hard Level:
11. 🔥 **Trapping Rain Water** - Opposite direction with preprocessing
12. 🔥 **Minimum Window Substring** - Advanced sliding window
13. 🔥 **Longest Substring with At Most K Distinct** - Sliding window

---

## 🎓 Complete Example: 3Sum Problem

**Problem:** Find all unique triplets that sum to zero.

```python
def three_sum(nums):
    """
    nums = [-1, 0, 1, 2, -1, -4]
    Answer: [[-1, -1, 2], [-1, 0, 1]]
    """
    nums.sort()  # IMPORTANT: Sort first!
    result = []
    
    for i in range(len(nums) - 2):
        # Skip duplicates for first number
        if i > 0 and nums[i] == nums[i-1]:
            continue
        
        # Now use two pointers for remaining two numbers
        left = i + 1
        right = len(nums) - 1
        target = -nums[i]  # We want nums[left] + nums[right] = target
        
        while left < right:
            current_sum = nums[left] + nums[right]
            
            if current_sum == target:
                result.append([nums[i], nums[left], nums[right]])
                
                # Skip duplicates for second number
                while left < right and nums[left] == nums[left + 1]:
                    left += 1
                # Skip duplicates for third number
                while left < right and nums[right] == nums[right - 1]:
                    right -= 1
                
                left += 1
                right -= 1
                
            elif current_sum < target:
                left += 1
            else:
                right -= 1
    
    return result

# Test
print(three_sum([-1, 0, 1, 2, -1, -4]))
# Output: [[-1, -1, 2], [-1, 0, 1]]
```

**Why this works:**
1. Sort array first (required for two pointers)
2. Fix first number, use two pointers for other two
3. Skip duplicates to avoid duplicate triplets
4. Time complexity: O(n²) instead of O(n³)!

---

## 🚀 Pro Tips

### 1. **Always Ask: Is the array sorted?**
- If YES → Consider opposite direction pointers
- If NO → Can I sort it? Or use same direction?

### 2. **Draw It Out**
- Visualize pointers moving
- Track variables on paper
- See the pattern emerge

### 3. **Start Simple**
- Solve brute force first
- Then optimize with two pointers
- Compare time complexity

### 4. **Common Mistakes**
❌ Forgetting to handle duplicates  
❌ Wrong boundary conditions (left < right vs left <= right)  
❌ Moving both pointers when should move one  
❌ Not considering edge cases (empty array, single element)

### 5. **Time Complexity**
- Two pointers: **O(n)** linear time
- Nested loops: **O(n²)** quadratic time
- **Big improvement** over brute force!

---

## 📊 Complexity Cheat Sheet

| Approach | Time | Space | When to Use |
|----------|------|-------|-------------|
| Opposite Direction | O(n) | O(1) | Sorted, pairs, palindrome |
| Same Direction | O(n) | O(1) | Remove, partition, in-place |
| Sliding Window (Fixed) | O(n) | O(1) | Fixed size subarray |
| Sliding Window (Variable) | O(n) | O(k) | Variable window with constraints |

---

## 🎯 Key Takeaways

1. **Two Pointers reduces time complexity** from O(n²) to O(n)
2. **Three main patterns:** Opposite, Same Direction, Sliding Window
3. **Look for keywords:** sorted, pair, subarray, consecutive, in-place
4. **Always consider:** boundary conditions, duplicates, edge cases
5. **Practice makes perfect:** Start with easy problems, build up

---

## 🏁 Next Steps

1. ✅ **Practice Template Problems** (Start with Easy level above)
2. ✅ **Solve 5 problems** of each pattern type
3. ✅ **Time yourself** - aim to solve in 15-20 minutes
4. ✅ **Explain your solution** out loud (interview practice)
5. ✅ **Move to Medium** level once comfortable

**Remember:** Two pointers is just a tool. Master when and how to use it, and you'll solve problems much faster!

Good luck! 🚀
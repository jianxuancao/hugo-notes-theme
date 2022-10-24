---
# Title, summary, and page position.
title: Array
linktitle: Array
summary: Notes on data structures.
weight: 2

# Page metadata.
date: '2018-09-09T00:00:00Z'
type: book # Do not modify.
toc: True
---

### 有序数组去重

[**力扣**](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)

```Java
public int removeDuplicates(int[] nums) {
	int slow = 0, fast = 0;

	while(fast < nums.length){
		if(nums[slow] != nums[fast]){  
        //如果想移除特定元素if(nums[fast] != val){ 
			slow++;
			nums[slow] = nums[fast];
		}
		fast++;
	}
	return slow+1;
}
```

### 移动0到末尾

[**力扣**](https://leetcode.cn/problems/move-zeroes/)

```Java
public void moveZeroes(int[] nums) {
	int slow = 0, fast = 0;

	while(fast < nums.length){
		if(nums[fast] != 0){
			nums[slow] = nums[fast];
			slow++;
		}
		fast++;
	}

	for(int i = slow; i < nums.length; i++){ 
		nums[i] = 0; // 把剩下的内容都变成0
	}
}
```

### 找出两数之和的位置 

[**力扣**](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/)

```Java
public int[] twoSum(int[] numbers, int target) {// 快慢指针
	int slow = 0, fast = 1;

    while(slow < numbers.length - 1){
        if(numbers[slow] + numbers[fast] == target){
            numbers[slow] = numbers[fast];
            return new int[]{slow + 1, fast + 1};
        }
        if(fast == numbers.length - 1){
            slow++;
            fast = slow + 1;
            continue;
        }
        fast++;
    }
    return new int[]{-1, -1};
}

int[] twoSum(int[] nums, int target) {  //左右指针
	//这个方法会快很多，因为快慢指针本质上是穷举直到找到答案
	//左右是根据sum的偏大还是偏小动态调整
    int left = 0, right = nums.length - 1;
    while (left < right) {
        int sum = nums[left] + nums[right];
        if (sum == target) {
            return new int[]{left + 1, right + 1};
        } else if (sum < target) {  //偏小了就让左边进一位
            left++; // 让 sum 大一点
        } else if (sum > target) {  //偏大了就让右边退一位
            right--; // 让 sum 小一点
        }
    }
    return new int[]{-1, -1};
}
```

### 反转数组

[**力扣**](https://leetcode.cn/problems/reverse-string/)

```Java
public void reverseString(char[] s) {
	int left = 0, right = s.length - 1;
    //左右分别从数组两端出发，互相交换，直到两个指针碰面
	while(left < right){
		char leftTemp = s[left];
		s[left] = s[right];
		s[right] = leftTemp;
		left++;
		right--;
	}
}
```

### 回文串判断

从中心向两端扩散

```Java
boolean isPalindrome(String s) {
    int left = 0, right = s.length() - 1;
    while (left < right) {
        if (s.charAt(left) != s.charAt(right)) {
            return false;
        }
        left++;
        right--;
    }
    return true;
}
```

### 最长回文子串

[**力扣**](https://leetcode.cn/problems/longest-palindromic-substring/)

向两边展开是合理的思路，每一位都尝试一下向两侧展开，寻找出最长的一次结果

```Java
public String longestPalindrome(String s) {
	String res = "";
    for (int i = 0; i < s.length(); i++) {
        String s1 = palindrome(s, i, i); //以s[i]为中心的最长回文子串
        String s2 = palindrome(s, i, i + 1); //s[i+1]为中心的最长回文子串
        //找出以s[i]和s[i+1]为中心最长的一个
        res = res.length() > s1.length() ? res : s1;
        res = res.length() > s2.length() ? res : s2;
    }
    return res;
}
String palindrome(String s, int l, int r) {
	while(l >= 0 && r < s.length() && s.charAt(l) == s.charAt(r)){
		l--;
		r++;
	}
	return s.substring(l + 1, r);
}
```

### 滑动窗口算法框架

```Java
void slidingWindow(String s) {
    HashMap<Character, Integer> window;
    
    int left = 0, right = 0;
    while (right < s.size()) {
        char c = s[right];// c 是将移入窗口的字符
        right++;// 增大窗口

        ...// 进行窗口内数据的一系列更新

        System.out.print("window: [%d, %d]\n", left, right); //debug 输出的位置
        
        while () { // 判断左侧窗口是否要收缩
            char d = s[left];// d 是将移出窗口的字符
            left++; // 缩小窗口

            ...// 进行窗口内数据的一系列更新  
        }
    }
}
```

### 最小覆盖子串

[**力扣**](https://leetcode.cn/problems/minimum-window-substring/)

```Java
public String minWindow(String s, String t) {
    HashMap<Character, Integer> window= new HashMap<>();
    HashMap<Character, Integer> need= new HashMap<>(); 
    
    for (int i = 0; i < t.length(); i++) {  // store target, <char, num of occrance of that char>
        char c = t.charAt(i);
        need.put(c, need.getOrDefault(c, 0) + 1);
    }

    int left = 0, right = 0;
    int valid = 0; // 合规字符数量
    int ansL = 0, min = Integer.MAX_VALUE;
    while (right < s.length()) {
        char c = s.charAt(right);
        right++;
        
        // 进行窗口内数据的一系列更新
        if (need.containsKey(c)) { //如果当前字符是需要的
            window.put(c, window.getOrDefault(c, 0) + 1); // 滑动窗口更新，储存该字符出现的次数，如果window里没有，那默认值是0
            if (window.get(c) == need.get(c)){ //字符出现的次数如果对应上了就加一个，如果需要aa（2次），而window.get(a)是1，那不加
                valid++; 
            }
        }

        while (valid == need.size()) { // 如果全部t都包含了(valid等于size)，开始缩小窗口
            if (right - left < min) { // 如果有更小的，更新min
                ansL = left;
                min = right - left;
            }

            char d = s.charAt(left);
            left++; // 缩小窗口
            if (need.containsKey(d)) { //如果删去的左边是需要的
                if (window.get(d) == need.get(d)){ // 
                    valid--; //目前窗口不满足最小覆盖，循环结束
                }
                window.put(d, window.get(d) - 1);
            }                    
        }
    }

    return min == Integer.MAX_VALUE ? "" : s.substring(ansL, ansL + min);
}


```

### 字符串的排列 

[**力扣**](https://leetcode.cn/problems/permutation-in-string/)

```Java
public boolean checkInclusion(String t, String s) {
    HashMap<Character, Integer> window = new HashMap<>();
    HashMap<Character, Integer> need = new HashMap<>(); 
    // store target, <char, num of occrance of that char>
    for (int i = 0; i < t.length(); i++) {
        char c = t.charAt(i);
        need.put(c, need.getOrDefault(c, 0) + 1);
    }

    int left = 0, right = 0;
    int valid = 0; // 合规字符数量
    int ansL = 0;
    while (right < s.length()) {
        char c = s.charAt(right);
        right++;

        // 计算window现在有几个符合条件的char
        //因为每次只添加一个char，比较一次就行
        if (need.containsKey(c)) { //如果是需要的，加入window里
            window.put(c, window.getOrDefault(c, 0) + 1); 
            // 滑动窗口更新
            if (window.get(c).equals(need.get(c))){ // 同样字符出现次数也是考虑标准
                valid++; //字符和出现次数对应上了就加一个
            }
        }

        if (right - left >= t.length()) { // 如果全部t都包含了(valid等于size)，开始缩小窗口
            if (valid == need.size()) { // 如果有更小的，更新指针
                return true;
            }
            char d = s.charAt(left);
            left++;
            
            if (need.containsKey(d)) { 
                if (window.get(d) == need.get(d)){
                    valid--;
                }
                window.put(d, window.get(d) - 1);
            }                    
        }
    }

    return false;
}
```

### 找到所有字母排列 

[**力扣**](https://leetcode.cn/problems/find-all-anagrams-in-a-string/)

```Java
public List<Integer> findAnagrams(String s, String p) {
    HashMap<Character, Integer> window = new HashMap<>();
    HashMap<Character, Integer> need = new HashMap<>(); 
    // store target, <char, num of occrance of that char>
    for (int i = 0; i < p.length(); i++) {
        char c = p.charAt(i);
        need.put(c, need.getOrDefault(c, 0) + 1);
    }

    int left = 0, right = 0;
    int valid = 0; // 合规字符数量
    List<Integer> ans = new ArrayList<Integer>();
    while (right < s.length()) {
        char c = s.charAt(right);
        right++;

        // 计算window现在有几个符合条件的char
        //因为每次只添加一个char，比较一次就行
        if (need.containsKey(c)) { //如果是需要的，加入window里
            window.put(c, window.getOrDefault(c, 0) + 1); 
            // 滑动窗口更新
            if (window.get(c).equals(need.get(c))){ // 同样字符出现次数也是考虑标准
                valid++; //字符和出现次数对应上了就加一个
            }
        }

        if (right - left >= p.length()) { // 如果全部t都包含了(valid等于size)，开始缩小窗口
            if (valid == need.size()) { // 如果有更小的，更新指针
                ans.add(left); //保存指针
            }
            char d = s.charAt(left);
            left++;
            
            if (need.containsKey(d)) { //如果缩小的一个字符是target的一部分
                if (window.get(d) == need.get(d)){ // 出现次数更改后就不valid了
                    valid--;
                }
                window.put(d, window.get(d) - 1); //减少一个出现次数
            }                    
        }
    }

    return ans;
}
```

### 字符串的排列

[**力扣**](https://leetcode.cn/problems/permutation-in-string/)

```Java
public boolean checkInclusion(String t, String s) {
    HashMap<Character, Integer> window = new HashMap<>();
    HashMap<Character, Integer> need = new HashMap<>(); 
    // store target, <char, num of occrance of that char>
    for (int i = 0; i < t.length(); i++) {
        char c = t.charAt(i);
        need.put(c, need.getOrDefault(c, 0) + 1);
    }

    int left = 0, right = 0;
    int valid = 0; // 合规字符数量
    int ansL = 0;
    while (right < s.length()) {
        char c = s.charAt(right);
        right++;

        // 计算window现在有几个符合条件的char
        //因为每次只添加一个char，比较一次就行
        if (need.containsKey(c)) { //如果是需要的，加入window里
            window.put(c, window.getOrDefault(c, 0) + 1); 
            // 滑动窗口更新
            if (window.get(c).equals(need.get(c))){ // 同样字符出现次数也是考虑标准
                valid++; //字符和出现次数对应上了就加一个
            }
        }

        if (right - left >= t.length()) { // 如果全部t都包含了(valid等于size)，开始缩小窗口
            if (valid == need.size()) { // 如果有更小的，更新指针
                return true;
            }
            char d = s.charAt(left);
            left++;
            
            if (need.containsKey(d)) {   //如果缩小的一个字符是target的一部分
                if (window.get(d) == need.get(d)){ // 出现次数更改后就不valid了
                    valid--;
                }
                window.put(d, window.get(d) - 1); 
            }                    
        }
    }

    return false;
}
```

### 找到所有字母排列 

[**力扣**](https://leetcode.cn/problems/find-all-anagrams-in-a-string/)

```Java
public List<Integer> findAnagrams(String s, String p) {
    HashMap<Character, Integer> window = new HashMap<>();
    HashMap<Character, Integer> need = new HashMap<>(); 
    // store target, <char, num of occrance of that char>
    for (int i = 0; i < p.length(); i++) {
        char c = p.charAt(i);
        need.put(c, need.getOrDefault(c, 0) + 1);
    }
    int left = 0, right = 0;
    int valid = 0; // 合规字符数量
    List<Integer> ans = new ArrayList<Integer>();
    while (right < s.length()) {
        char c = s.charAt(right);
        right++;
        // 计算window现在有几个符合条件的char
        //因为每次只添加一个char，比较一次就行
        if (need.containsKey(c)) { //如果是需要的，加入window里
            window.put(c, window.getOrDefault(c, 0) + 1); 
            // 滑动窗口更新
            if (window.get(c).equals(need.get(c))){ // 同样字符出现次数也是考虑标准
                valid++; //字符和出现次数对应上了就加一个
            }
        }
        if (right - left >= p.length()) { // 如果全部t都包含了(valid等于size)，开始缩小窗口
            if (valid == need.size()) { // 如果有更小的，更新指针
                ans.add(left); //保存指针
            }
            char d = s.charAt(left);
            left++;
            
            if (need.containsKey(d)) { 
                if (window.get(d) == need.get(d)){
                    valid--;
                }
                window.put(d, window.get(d) - 1);
            }                    
        }
    }
    return ans;
}
```

### 无重复字符的最长子串 

[**力扣**](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

```Java
public int lengthOfLongestSubstring(String s) {
    HashSet<Character> window = new HashSet<>();
    int left = 0, right = 0;
    int result = 0;
    while (right < s.length()) {
        char c = s.charAt(right);
        right++;
    
        while (window.contains(c)) { //如果重复出现,收紧左侧指针
            window.remove(s.charAt(left));
            left++;//左进一
        }
        window.add(c);
        System.out.println(window + " " + left + " " + right);
        result = Math.max(result, right - left);
    }

    return result;
}
```

### 二分查找 

[**力扣**](https://leetcode.cn/problems/binary-search/)

```Java
public int search(int[] nums, int target) {
    int left = 0 ;
    int right = nums.length - 1;

    while (right >= left){
        int mid = left + (right - left) / 2;
        if(nums[mid] == target){
            return mid;
        }else if(nums[mid] > target){
            right = mid - 1;
        }else if(nums[mid] < target){
            left = mid + 1;
        }
    }
    return -1;
}
```

### **左侧边界**

```Java
int left_bound(int[] nums, int target) {
    // 搜索区间为 [left, right]
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            // 搜索区间变为 [mid+1, right]
            left = mid + 1;
        } else if (nums[mid] > target) {
            // 搜索区间变为 [left, mid-1]
            right = mid - 1;
        } else if (nums[mid] == target) {
            // 收缩右侧边界
            right = mid - 1;
        }
    }
    // 判断 target 是否存在于 nums 中
    // 此时 target 比所有数都大，返回 -1
    if (left == nums.length) return -1;
    // 判断一下 nums[left] 是不是 target
    return nums[left] == target ? left : -1;
}
```

### **右侧边界**

```Java
int right_bound(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else if (nums[mid] == target) {
            // 这里改成收缩左侧边界即可
            left = mid + 1;
        }
    }
    // 最后改成返回 left - 1
    if (left - 1 < 0) return -1;
    return nums[left - 1] == target ? (left - 1) : -1;
}
```

### 按权重随机选择 

[**力扣**](https://leetcode.cn/problems/random-pick-with-weight)

```Java
class Solution {
    private int[] preSum;
    private Random rand = new Random();
    
    public Solution(int[] w) {
        int n = w.length;
        // 构建前缀和数组，偏移一位留给 preSum[0]
        preSum = new int[n + 1];
        preSum[0] = 0;
        // preSum[i] = sum(w[0..i-1])
        for (int i = 1; i <= n; i++) {
            preSum[i] = preSum[i - 1] + w[i - 1];
        }
    }
    
    public int pickIndex() {
        int n = preSum.length;
        // 在[0, preSum[n - 1](exclusive)] 中随机选择一个数字, +1恢复为[1, preSum[n - 1]]
        int target = rand.nextInt(preSum[n - 1]) + 1;
        // 获取 target 在前缀和数组 preSum 中的索引
        // 别忘了前缀和数组 preSum 和原始数组 w 有一位索引偏移
        return left_bound(preSum, target) - 1;
    }

    int left_bound(int[] nums, int target) {
        if (nums.length == 0) return -1;
        int left = 0, right = nums.length;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] == target) {
                right = mid;
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else if (nums[mid] > target) {
                right = mid;
            }
        }
        return left;
    }
}
```

### **田忌赛马**

```Java
int[] advantageCount(int[] nums1, int[] nums2) {
    int n = nums1.length;
    // 给 nums2 降序排序
    PriorityQueue<int[]> maxpq = new PriorityQueue<>(
        (int[] pair1, int[] pair2) -> { 
            return pair2[1] - pair1[1];
        }
    );
    for (int i = 0; i < n; i++) {
        maxpq.offer(new int[]{i, nums2[i]});
    }
    // 给 nums1 升序排序
    Arrays.sort(nums1);

    // nums1[left] 是最小值，nums1[right] 是最大值
    int left = 0, right = n - 1;
    int[] res = new int[n];

    while (!maxpq.isEmpty()) {
        int[] pair = maxpq.poll();
        // maxval 是 nums2 中的最大值，i 是对应索引
        int i = pair[0], maxval = pair[1];
        if (maxval < nums1[right]) {
            // 如果 nums1[right] 能胜过 maxval，那就自己上
            res[i] = nums1[right];
            right--;
        } else {
            // 否则用最小值混一下，养精蓄锐
            res[i] = nums1[left];
            left++;
        }
    }
    return res;
}
```
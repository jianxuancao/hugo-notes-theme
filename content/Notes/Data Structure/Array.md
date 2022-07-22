---
# Title, summary, and page position.
title: LinkedList
linktitle: Array
summary: Notes on data structures.
weight: 2

# Page metadata.
date: '2018-09-09T00:00:00Z'
type: book # Do not modify.
toc: false
---

[**有序数组去重**](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)
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


[**移动0到末尾**](https://leetcode.cn/problems/move-zeroes/)
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


[**找出两数之和的位置**](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/)
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


[**反转数组**](https://leetcode.cn/problems/reverse-string/)
```Java
public void reverseString(char[] s) {
	int left = 0, right = s.length - 1;
	while(left < right){
		char leftTemp = s[left];
		s[left] = s[right];
		s[right] = leftTemp;
		left++;
		right--;
	}
}
```


[**回文串判断**]()
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


[**最长回文子串**](https://leetcode.cn/problems/longest-palindromic-substring/)
向两边展开是合理的思路
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


[**滑动窗口算法框架**]()
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

[**最小覆盖子串**](https://leetcode.cn/problems/minimum-window-substring/)
```Java
public String minWindow(String s, String t) {
    HashMap<Character, Integer> window= new HashMap<>();
    HashMap<Character, Integer> need= new HashMap<>(); 
    // store target, <char, num of occrance of that char>
    for (int i = 0; i < t.length(); i++) {
        char c = t.charAt(i);
        need.put(c, need.getOrDefault(c, 0) + 1);
    }

    int left = 0, right = 0;
    int valid = 0; // 合规字符数量
    int ansL = 0, min = Integer.MAX_VALUE;
    while (right < s.length()) {
        char c = s.charAt(right);
        right++;
        
        if (need.containsKey(c)) { // 进行窗口内数据的一系列更新
            window.put(c, window.getOrDefault(c, 0) + 1); // 滑动窗口更新
            if (window.get(c) == need.get(c)){ //字符对应上了就加一个
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


[**字符串的排列**](https://leetcode.cn/problems/permutation-in-string/)
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


[**找到所有字母排列**](https://leetcode.cn/problems/find-all-anagrams-in-a-string/)
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
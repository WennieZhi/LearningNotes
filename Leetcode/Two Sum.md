#### Two Sum

#####暴力破解

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return new int[2];
        }
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[i] == target - nums[j]) {
                    return new int[] {i, j};
                }
            } 
        }
        return new int[2];
    }
}
```

* Check Input

  ```java
  if (nums == null || nums.length == 0) {
      return new int[2];
  }
  ```

  * 判断数组为空的写法

    ```java
    nums == null
    nums.length == 0
    ```

  * return空数组的写法

    ```java
    return new int[2];
    ```

* return 有默认值得数据的写法

```java
return new int[] {i, j};
```

* i + j = target --> i = target - j

​    防止i + j溢出，所以采用减的写法。



##### HashMap解法

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return new int[2];
        }
        Map<Integer,Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            if (map.containsKey(nums[i])) {               
                return new int[] {map.get(nums[i]), i};                    
            }              
            map.put(target - nums[i], i);
        } 
        return new int[2];
    }
}
```

* 用Map声明变量

  Map是一个接口，用其声明的变量则表示它拥有这个接口的方法。

  new一个对象的时候则必须声明一个具体的类。

```java
Map<Integer,Integer> map = new HashMap<>();
```

  这样的好处是更加地灵活，这在List的声明中体现地更加突出。

  比如一个用List声明变量，那么这个变量可以作为LinkedList的引用，

  也可以作为ArrayList的引用。

* 有一个bug你没有发现

  ```java
  class Solution {
      public int[] twoSum(int[] nums, int target) {
          if (nums == null || nums.length == 0) {
              return new int[2];
          }
          Map<Integer,Integer> map = new HashMap<>();
          for (int i = 0; i < nums.length; i++) {
              map.put(target - nums[i], i);
          } 
          for (int i = 0; i < nums.length; i++) {
              if (map.containsKey(nums[i])) {
                  return new int[] {map.get(nums[i]),i};
              }
          }
          return new int[2];
      }
  }
  ```

  input 为 [1,2] , target = 2的时候，会返回 [0,0]

  所以需要去判断 map.get(nums[i]) != i

* 还有一个bug你没有发现

  ```java
  class Solution {
      public int[] twoSum(int[] nums, int target) {
          if (nums == null || nums.length == 0) {
              return new int[2];
          }
          Map<Integer,Integer> map = new HashMap<>();
          for (int i = 0; i < nums.length; i++) {
              map.put(target - nums[i], i);
              if (map.containsKey(nums[i]) && map.get(nums[i]) != i) {               
                  return new int[] {map.get(nums[i]),i};                    
              }   
          } 
          return new int[2];
      }
  }
  ```

  input 为 [3,3] , target = 6的时候，会返回 [0,0]

  最开始是(3, 0)写到map里面，之后要写(3, 1)，就把0 overwrite为 1 了。

  一直走到最后的return new int[2], 也就是[0,0]

  这里顺便说一下为什么要return一个0数组而不是一个null。考虑到包容性，我返回一个合法的[0,0]数组不会引起别人程序出错而run不下去。但是当我返回null时，别人还需要专门去判断是否为null值，较为麻烦。

* 如何改进更好？

  其实自己想一想，为什么不能先检查再put呢？

  这样的话我就省去了 map.get(nums[i]) != i的判断，因为元素i在检查的时候还没有put进去，所以肯定不是自己。

  也省去了第二个bug，因为当出现target - item1 = item1 = item2的情况下，我也是在put之前就检测到了，根本不用去写map

  所以最终修改的代码如下

  ```java
  class Solution {
      public int[] twoSum(int[] nums, int target) {
          if (nums == null || nums.length == 0) {
              return new int[2];
          }
          Map<Integer,Integer> map = new HashMap<>();
          for (int i = 0; i < nums.length; i++) {
              if (map.containsKey(nums[i])) {               
                  return new int[] {map.get(nums[i]), i};                    
              }              
              map.put(target - nums[i], i);
          } 
          return new int[2];
      }
  }
  ```


##### 相向双指针解法（适用于从大到小递增的数组）

* 左右两个指针，相向而行。

  ```java
  int left = 0; 
  int right = nums.length - 1; 
  ```

  * 如果nums[left] + nums[right] < target，则说明left指向的这个元素太小了，即使加上数组中最大的那个数，也不能满足相加等于target的情况。所以要增大left，指向下一个元素。

  * 如果nums[left] + nums[right] > target, 则说明right指向的这个元素太大了，要减小他，所以要使right向左移动。
  * 如果nums[left] + nums[right] = target，return。
  * 注意while循环的判断条件，是两个指针相遇的情况。即 left < right

  ```java
  class Solution {
      public int[] twoSum(int[] nums, int target) {
          if (nums == null || nums.length == 0) {
              return new int[2];
          }
          Arrays.sort(nums);
          int left = 0;
          int right = nums.length - 1;
          while (left < right) {
              if (nums[left] == target - nums[right]) {
                  return new int[] {left, right};
              } else if (nums[left] < target - nums[right]) {
                  left++;
              } else {
                  right--;
              }
          }
          return new int[2];
      }
  }
  ```



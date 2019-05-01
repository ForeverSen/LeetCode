对LeetCode中的有关数学问题进行了刷题总结：
#### 素数：
[204. Count Primes](https://leetcode.com/problems/count-primes/description/)
##### 题目描述：
统计所有小于非负整数 n 的质数的数量。
示例:

输入: 10
输出: 4
解释: 小于 10 的质数一共有 4 个, 它们是 2, 3, 5, 7 。

##### 解题思路：
本题要使用埃拉托斯特尼筛法，这样运行不会超时。

要得到自然数n以内的全部素数，必须把不大于根号n的所有素数的倍数剔除，剩下的就是素数。
给出要筛数值的范围n，找出以内的素数。先用2去筛，即把2留下，把2的倍数剔除掉；再用下一个质数，也就是3筛，把3留下，把3的倍数剔除掉；接下去用下一个质数5筛，把5留下，把5的倍数剔除掉；不断重复下去......。
~~~ java
class Solution {
    public int countPrimes(int n) {
        boolean [] notPrim = new boolean[n];
        int count = 0;
        for (int i = 2; i < n; i++){
            if (notPrim[i] == false) {
                count++;
                for (int j = 2; j*i < n; j++) {
                    notPrim[j*i] = true;
                }
            }
        }
        return count;
    }
}
~~~

#### 十六进制
[405. Convert a Number to Hexadecimal](https://leetcode.com/problems/convert-a-number-to-hexadecimal/description/)
##### 题目描述：
给定一个整数，编写一个算法将这个数转换为十六进制数。对于负整数，我们通常使用 补码运算 方法。

注意:

- 十六进制中所有字母(a-f)都必须是小写。
- 十六进制字符串中不能包含多余的前导零。如果要转化的数为0，那么以单个字符'0'来表示；对于其他情况，十六进制字符串中的第一个字符将不会是0字符。 
- 给定的数确保在32位有符号整数范围内。
- 不能使用任何由库提供的将数字直接转换或格式化为十六进制的方法。

~~~
Example 1:

Input:
26
Output:
"1a"

Example 2:
Input:
-1
Output:
"ffffffff"
~~~
##### 解题思路：
- 使用位运算，每4位，对应1位16进制数字。
- 将输入右移4位，再做一次，直到输入变为0

这里两个概念了解下：
- 位与(&)：第一个操作数的的第n位于第二个操作数的第n位如果都是1，那么结果的第n为也为1，否则为0。
- 无符号右移：位运算符（>>>），它使用了“零扩展”：无论正负，都在高位插入0。
~~~ java
class Solution {
    public String toHex(int num) {
        char [] array = {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'a', 'b', 'c', 'd', 'e', 'f'};
        if (num == 0) {
            return "0";
        }
        StringBuilder sb = new StringBuilder();
        while (num != 0) {
            sb.append(array[num & 15]);//使用位与
            num = num >>> 4;	//无符号右移，左边填0
        }
        return sb.reverse().toString();
    }
}
~~~
#### 阶乘问题
[172. Factorial Trailing Zeroes](https://leetcode.com/problems/factorial-trailing-zeroes/)
##### 题目描述：
给定一个整数 n，返回 n! 结果尾数中零的数量。
~~~ 
Example 1:

Input: 3
Output: 0
Explanation: 3! = 6, no trailing zero.

Example 2:

Input: 5
Output: 1
Explanation: 5! = 120, one trailing zero.
~~~
##### 解题思路：
首先题目的意思是末尾有几个0
 比如6! = 【1* 2* 3* 4* 5* 6】， 其中只有2*5末尾才有0，所以就可以抛去其他数据 专门看2和5以及其倍数，毕竟 4 * 25末尾也是0
 2肯定比5多 所以只算5的个数就行了。

 对于一个数 N，它所包含 5 的个数为：N/5 + N/25 + N/75 + ...。
~~~ java
class Solution {
    public int trailingZeroes(int n) {
        return n == 0 ? 0 : n / 5 + (trailingZeroes(n / 5));
    }
}
~~~

#### 字符串加法减法：
即两个字符串相加问题，但是不能直接转成Integer解决，解决此类问题一般思路是从右到左开始遍历两个数，如果进位就加一，这个时候就要定义一个专门存储进位的变量count。


**1、字符串加法：**
[415. Add Strings](https://leetcode.com/problems/add-strings/)
##### 题目描述：
给定两个字符串形式的非负整数 num1 和num2 ，计算它们的和。

注意：

num1 和num2 的长度都小于 5100.
num1 和num2 都只包含数字 0-9.
num1 和num2 都不包含任何前导零。
你不能使用任何內建 BigInteger 库， 也不能直接将输入的字符串转换为整数形式。

##### 解题思路：
~~~ java
class Solution {
    public String addStrings(String num1, String num2) {
        StringBuilder sb = new StringBuilder();
        int n1 = num1.length() - 1;
        int n2 = num2.length() - 1;
        int count = 0;	//表示进位数
        while (count == 1 || n1 >= 0 || n2 >= 0) {
            int x = n1 < 0 ? 0 : num1.charAt(n1--) - '0';
            int y = n2 < 0 ? 0 : num2.charAt(n2--) - '0';
            sb.append((x + y + count) % 10);
            count = (x + y + count) / 10;
        }
        return sb.reverse().toString();
    }
}
~~~


**2、二进制加法：**

[67. Add Binary](https://leetcode.com/problems/add-binary/)
##### 题目描述：
给定两个二进制字符串，返回他们的和（用二进制表示）。

输入为非空字符串且只包含数字 1 和 0。
~~~
Example 1:

Input: a = "11", b = "1"
Output: "100"

Example 2:

Input: a = "1010", b = "1011"
Output: "10101"
~~~
##### 解题思路：
将两个数从右往左进行相加，用变量count来判断是不是往前面进一位，即进位。
~~~ java
class Solution {
    public String addBinary(String a, String b) {
        int aNum = a.length() - 1;
        int bNum = b.length() - 1;
        int count = 0;
        StringBuilder sb = new StringBuilder();
        
        // 注意这里count==1也是条件，即当aNum和bNum都为0时向前进一时
        while (count == 1 || aNum >= 0 || bNum >= 0) {
            if (aNum >= 0 && a.charAt(aNum) == '1') {
                count++;
               
            }
            if (bNum >= 0 && b.charAt(bNum) == '1') {
                count++;
            }
            //每次都要往前移一位
            aNum--; 
            bNum--;
            
            sb.append(count % 2);
            count = count / 2;
        }
        return sb.reverse().toString();
    }      
}
~~~
#### 数组中出现次数多于 n / 2 的元素：

[169. Majority Element](https://leetcode.com/problems/majority-element/description/)
##### 题目描述：
给定大小为n的数组，找到多数元素。 多数元素是出现超过⌊n /2⌋倍的元素。
您可以假设该数组非空，并且多数元素始终存在于数组中。
~~~ 
Example 1:
Input: [3,2,3]
Output: 3

Example 2:
Input: [2,2,1,1,1,2,2]
Output: 2
~~~
##### 解题思路：
1、可以先对数组排序，最中间那个数出现次数一定多于 n / 2。
~~~ java
class Solution {
    public int majorityElement(int[] nums) {
        Arrays.sort(nums);
        return (nums[nums.length / 2]);
    }
}
~~~

2、使用 摩尔投票算法。那么我们就先从数组的第一个元素开始，假定它代表的群体的人数是最多的，那么根据**数组中出现次数超过一半的数只有一个**的特质，如果我们设置一个计数器count，在遍历时遇到不同于这个群体的人时就将计数器-1，遇到同个群体的人时就+1，只要在计数器归0时就重新假定当前元素代表的群体为人数最多的群体再继续遍历，那么到了最后，计数器记录的那个群体必定是人最多的那个群体。这里就使得元素排序是不会造成任何影响的，只关心元素的个数所带来的对于计数器+1或-1的影响。
~~~ java
class Solution {
    public int majorityElement(int[] nums) {
        int count = 0, majority = 0;
       for (int num : nums) {
           if (count == 0) {
               majority = num;
           }
           if (num != majority) {
               count--;
           }
           else {
               count++;
           }
       }
        return majority;
    }
}
~~~

#### 相遇问题：
[462. Minimum Moves to Equal Array Elements II](https://leetcode.com/problems/minimum-moves-to-equal-array-elements-ii/)
##### 题目描述：
给定一个非空整数数组，找出使所有数组元素相等所需的最小移动数，其中移动使选定的元素递增1或递减1。您可以假设数组的长度最多为10,000。
~~~
Input:
[1,2,3]

Output:
2

Explanation:
Only two moves are needed (remember each move increments or decrements one element):

[1,2,3]  =>  [2,2,3]  =>  [2,2,2]
~~~
##### 解题思路：
典型相遇问题，**移动距离最小的方式是所有元素都移动到中位数**。另外本题也属于求TopK问题，也可以用基于切分的快速选择算法，也可以用优先队列来解决，最重要是要知道什么情况下移动距离才是最小的。
~~~ java
class Solution {
    public int minMoves2(int[] nums) {
        Arrays.sort(nums);
        int move = 0;
        int l = 0, h = nums.length - 1;
        while (l <= h) {
            move = move + nums[h] - nums[l];
            l++;
            h--;
        }
        return move;
    }
}
~~~

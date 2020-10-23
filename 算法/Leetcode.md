> 只出现的一次的数字：
>
> 题目描述：给定一个**非空**整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。
>
> 既满足时间复杂度又满足空间复杂度，就要提到位运算中的异或运算 XOR，主要因为异或运算有以下几个特点：
> 一个数和 0 做 XOR 运算等于本身：a⊕0 = a
> 一个数和其本身做 XOR 运算等于 0：a⊕a = 0
> XOR 运算满足交换律和结合律：a⊕b⊕a = (a⊕a)⊕b = 0⊕b = b
>
> ```java
> class Solution {
>     public int singleNumber(int[] nums) {
>         int result=0;
>         for(int i: nums){
>             result=result^i;
>         }
>         return result;
>     }
> }
> ```

>给定一个**非空**整数数组，除了某个元素只出现一次以外，其余每个元素均出现了三次。找出那个只出现了一次的元素。
>
>[只出现一次的数字 II](https://leetcode-cn.com/problems/single-number-ii/)
>
>```java
>class Solution {
>    public int singleNumber(int[] nums) {
>        int result=0;
>        for (int i=0 ; i<32; i++){
>            int count=0;
>            for(int j=0; j<nums.length; j++){
>                int temp = nums[j];
>                if((temp >>> i & 1 ) == 1){
>                    count++;
>                }
>            }
>            if(count % 3 != 0){
>                result = result | 1 << i ;
>            }
>        }
>        return result;
>    }
>}
>```

>
>
>字符串 `S` 由小写字母组成。我们要把这个字符串划分为尽可能多的片段，同一个字母只会出现在其中的一个片段。返回一个表示每个字符串片段的长度的列表。
>
>>
>>
>>输入：S = "ababcbacadefegdehijhklij"
>>输出：[9,7,8]
>>解释：
>>划分结果为 "ababcbaca", "defegde", "hijhklij"。
>>每个字母最多出现在一个片段中。
>>像 "ababcbacadefegde", "hijhklij" 的划分是错误的，因为划分的片段数较少。
>>
>>来源：力扣（LeetCode）
>>链接：https://leetcode-cn.com/problems/partition-labels
>>著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
>
>```java
>class Solution {
>    public List<Integer> partitionLabels(String S) {
>        LinkedHashMap<Byte,ArrayList<Integer>> hm= new LinkedHashMap<Byte, ArrayList<Integer>>();
>        byte[] bytes=S.getBytes();
>        //求每个字母的所在范围的并集；
>        for(int i=0 ; i<bytes.length ; i++){
>            byte temp=bytes[i];
>            if(hm.containsKey(temp)){
>                ArrayList<Integer> arr=hm.get(temp);
>                arr.set(1,i);
>            }else{
>                ArrayList<Integer> al=new ArrayList<Integer>();
>                al.add(i);
>                al.add(i);
>                hm.put(temp,al);
>            }
>        }
>        Set<Byte> set=hm.keySet();
>        //合并集合
>        //for(byte b: set){
>        //    ArrayList<Integer> arr = hm.get(b);
>        //    System.out.println("["+arr.get(0)+","+arr.get(1)+"]");
>        //}
>        ArrayList<Integer> result=new ArrayList<Integer>();
>        int s=0;
>        int e=0;
>        for(byte b: set){
>            ArrayList<Integer> arr = hm.get(b);
>            int temps=arr.get(0);
>            int tempe=arr.get(1);
>            if(e<temps){
>                result.add(e-s+1);
>                //result.add(s);
>                //result.add(e);
>                s=temps;
>                e=tempe;
>                continue;
>            }else if(e<tempe){
>                e=tempe;
>            }
>            //System.out.println("["+arr.get(0)+","+arr.get(1)+"]");
>        }
>        result.add(e-s+1);
>        //result.add(s);
>        //result.add(e);
>        //for(Integer i: result){
>        //    System.out.print(i);
>        //}
>
>        return result; 
>    }
>}
>```
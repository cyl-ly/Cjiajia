## [两个列表的最小索引总和](https://leetcode.cn/problems/minimum-index-sum-of-two-lists/)

> 假设 Andy 和 Doris 想在晚餐时选择一家餐厅，并且他们都有一个表示最喜爱餐厅的列表，每个餐厅的名字用字符串表示。
>
> 你需要帮助他们用**最少的索引和**找出他们**共同喜爱的餐厅**。 如果答案不止一个，则输出所有答案并且不考虑顺序。 你可以假设答案总是存在。

> ```
> 输入: list1 = ["Shogun", "Tapioca Express", "Burger King", "KFC"]，list2 = ["Piatti", "The Grill at Torrey Pines", "Hungry Hunter Steakhouse", "Shogun"]
> 输出: ["Shogun"]
> 解释: 他们唯一共同喜爱的餐厅是“Shogun”。
> ```
>
> ```
> 输入:list1 = ["Shogun", "Tapioca Express", "Burger King", "KFC"]，list2 = ["KFC", "Shogun", "Burger King"]
> 输出: ["Shogun"]
> 解释: 他们共同喜爱且具有最小索引和的餐厅是“Shogun”，它有最小的索引和1(0+1)。
> ```

#### 题解

```c++
class Solution {
public:
    vector<string> findRestaurant(vector<string>& list1, vector<string>& list2) {
        //哈希map存储名称和下标
        unordered_map<string, int> mp_1;
        unordered_map<string, int> mp_2;
        for(int i=0; i<list1.size(); i++){
            mp_1[list1[i]] = i;
        }
        for(int i=0 ;i<list2.size(); i++){
            mp_2[list2[i]] = i;
        }
        //开始遍历找
        vector<string> result;
        int sum = 2000;
        for(auto i:mp_1){
            if(mp_2.find(i.first) != mp_2.end()){//有相同爱好
                if(i.second + mp_2[i.first] < sum){
                    sum = i.second + mp_2[i.first];
                    result.clear();     //清空
                    result.push_back(i.first);
                }
                else if(i.second + mp_2[i.first] == sum)    //相同最小索引，都保存
                    result.push_back(i.first);
            }
        }
        return result;
    }
};
```


### 滑动窗口 + 哈希表
本题最朴素的想法为先使用**全排列**生成`words`的全排列，再在`s`中寻找遍历所有的全排列。但在测试的过程中发现，单纯使用`string.find`只能找到**第一个**匹配的位置，而测试集中存在多个匹配位置的情况，并且排列中存在**重复排列的问题**，这是`words`中单词不唯一的结果。需要在找到所有的`pos`后对`vector`进行双指针去重。但最后测试中出现了**超时的问题**。原因是：这样做的时间复杂度为$O(mn)$。

这里先思考两个问题：
> 1.能否像传统滑动窗口一样，将滑动操作的时间复杂度降低到$O(1)$?
> 2.题目为什么强调`words`数组中的单词长度是相等的?
基本的思想为：
> - 假设字符串`s`的长度为n
> - 假设`words`中的单词个数为$m$，每个单词的长度为$d$，所有单词的长度之和为$L$
> - 维护一个长度为$L$的窗口在字符串`s`上进行滑动，每次的步长为$d$
> - 每次滑动不用重新计算窗口内的单词，因为每次窗口滑动的步长为$d$，跨越了整个单词
> - **除了第一个单词离开了窗口，窗口内的其他单词均未收到影响**

经过上面的分析，需要设置$d-1$个起点，也即$d-1$个滑动窗口，每个窗口的每次滑动步长为$d$。可以进一步**剪枝**，不再计算初始窗口超出字符串`s`的窗口。

**代码**

*C++ version*
```
class Solution {
public:
    vector<int> findSubstring(string s, vector<string>& words) {
    int lengthS = s.size(), numOfWords = words.size(), lengthWord = words[0].size();
    unordered_map<string, int> need;
    int windowSize;
    for(const auto &s : words){
        windowSize += s.size();
        need[s]++;
    }
    vector<int> output;
    vector<unordered_map<string, int>> windows(lengthWord);
    for(int i = 0; i < lengthWord && i + windowSize <= lengthS; ++i){
        for(int j = i; j < i + windowSize; j += lengthWord){
            string token = s.substr(j, lengthWord);
            windows[i][token]++;
        }
        if(windows[i] == need){
            output.push_back(i);
        }
    }
    for(int i = lengthWord; i + windowSize <= lengthS; ++i){
        int r = i % lengthWord;
        string leftToken = s.substr(i - lengthWord, lengthWord);
        string rightToken = s.substr(i + windowSize - lengthWord, lengthWord);
        if(--windows[r][leftToken] == 0){
            windows[r].erase(leftToken);
        }
        windows[r][rightToken]++;
        if(windows[r] == need){
            output.push_back(i);
        }
    }
    return output;
}
};
```

*python version*
```
class Solution:
    def findSubstring(self, s: str, words: List[str]) -> List[int]:
        len_of_s, num_of_words, len_of_word = len(s), len(words), len(words[0])
        need = {}
        len_of_window = num_of_words * len_of_word
        for word in words:
            if need.get(word) == None:
                need[word] = 1
            else:
                need[word] += 1
        output = []
        windows = [{} for _ in range(len_of_word)]
        for i in range(len_of_word):
            if i + len_of_window > len_of_s:
                break
            for j in range(i, i + len_of_window, len_of_word):
                token = s[j:j + len_of_word]
                if windows[i].get(token) == None:
                    windows[i][token] = 1
                else:
                    windows[i][token] += 1
            if windows[i] == need:
                output.append(i)
        for i in range(len_of_word, len_of_s - len_of_window + 1):
            r = i % len_of_word
            left_token = s[i - len_of_word: i]
            right_token = s[i + len_of_window - len_of_word: i + len_of_window]
            if windows[r][left_token] == 1:
                windows[r].pop(left_token)
            else:
                windows[r][left_token] -= 1
            if windows[r].get(right_token) == None:
                windows[r][right_token] = 1
            else:
                windows[r][right_token] += 1
            if windows[r] == need:
                output.append(i)
        return output
```

**参考**
- [C++ map api](https://cplusplus.com/reference/map/map/)
- [递归实现全排列](https://blog.csdn.net/qq_42815188/article/details/109414659)
- [What does the == operator actually do on a Python dictionary?](https://stackoverflow.com/questions/17217225/what-does-the-operator-actually-do-on-a-python-dictionary)
- [Python Dictionary Methods](https://www.w3schools.com/python/python_dictionaries_methods.asp)
- [What’s the difference between “is” and “==” in Python?](https://towardsdatascience.com/whats-the-difference-between-is-and-in-python-dc26406c85ad)


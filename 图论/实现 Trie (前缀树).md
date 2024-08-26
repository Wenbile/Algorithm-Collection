## 实现 Trie (前缀树)
> https://leetcode.cn/problems/implement-trie-prefix-tree/description/?envType=study-plan-v2&envId=top-100-liked

Trie（发音类似 "try"）或者说 前缀树 是一种树形数据结构，用于高效地存储和检索字符串数据集中的键。这一数据结构有相当多的应用情景，例如自动补全和拼写检查。

请你实现 `Trie` 类：

`Trie()` 初始化前缀树对象。
`void insert(String word)` 向前缀树中插入字符串 `word` 。
`boolean search(String word)` 如果字符串 `word` 在前缀树中，返回 `true`（即，在检索之前已经插入）；否则，返回 `false` 。
`boolean startsWith(String prefix)` 如果之前已经插入的字符串 `word` 的前缀之一为 `prefix` ，返回 `true` ；否则，返回 `false` 。

示例:
> 输入
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
\[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
输出
[null, null, true, false, true, null, true]
解释
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // 返回 True
trie.search("app");     // 返回 False
trie.startsWith("app"); // 返回 True
trie.insert("app");
trie.search("app");     // 返回 True



```javascript

var Trie = function() {
    // 构建一个26字母的列表，value： 0未访问过 1已访问 2已访问且结尾 next：下一个字母的26字母列表
    this.list = new Array(26).fill(0).map(()=> ({value: 0, next: undefined}))
    this.charOffset = (char) => {
        return char.charCodeAt() - 'a'.charCodeAt()
    }
};

/** 
 * @param {string} word
 * @return {void}
 */
Trie.prototype.insert = function(word) {
    let tempList = this.list
    for(let i = 0; i < word.length; i++) {
        const charIndex = this.charOffset(word[i])
        const charInfo = tempList[charIndex]
        // 到结尾
        if(i === word.length - 1) {
            charInfo.value = 2
        } else {
            // 如果未访问过，定义为已访问
            if(charInfo.value === 0) {
                charInfo.value = 1
            }
            // 下一个字母
            if(!charInfo.next) {
                charInfo.next = new Array(26).fill(0).map(()=> ({value: 0, next: undefined}))
            }
            tempList = charInfo.next
        }
    }
};

/** 
 * @param {string} word
 * @return {boolean}
 */
Trie.prototype.search = function(word) {
    let tempList = this.list
    for(let i = 0; i < word.length; i++) {
        const charIndex = this.charOffset(word[i])
        const charInfo = tempList[charIndex]
        // 到结尾
        if(i === word.length - 1) {
            if(charInfo.value === 2) {
                return true
            }
        } else {
            if(charInfo.value !== 0 && charInfo.next) {
                tempList = charInfo.next
            } else {
                return false
            }
        }
    }
    return false
};

/** 
 * @param {string} prefix
 * @return {boolean}
 */
Trie.prototype.startsWith = function(prefix) {
    let tempList = this.list
    for(let i = 0; i < prefix.length; i++) {
        const charIndex = this.charOffset(prefix[i])
        const charInfo = tempList[charIndex]
        // 到结尾
        if(i === prefix.length - 1) {
            if(charInfo.value !== 0) {
                return true
            }
        } else {
            if(charInfo.value !== 0 && charInfo.next) {
                tempList = charInfo.next
            } else {
                return false
            }
        }
    }
    return false
};

/**
 * Your Trie object will be instantiated and called as such:
 * var obj = new Trie()
 * obj.insert(word)
 * var param_2 = obj.search(word)
 * var param_3 = obj.startsWith(prefix)
 */
```
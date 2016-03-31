---
layout: post
title: Trie字典树总结
author: 扬涛
tags: [Trie, ]  
---



### （一）字典树概念  

`字典树`——又称单词查找树，Trie树，是一种树形结构，是一种哈希树的变种。典型应用是用于统计，排序和保存大量的字符串（但不仅限于字符串），所以经常被搜索引擎系统用于文本词频统计。

它的优点是：利用字符串的公共前缀来减少查询时间，最大限度地减少无谓的字符串比较，查询效率比哈希树高。

![这里写图片描述](http://img.blog.csdn.net/20151025214627066)


有三种基本的性质：

* 根节点不包含字符，除根节点外每一个节点都只包含一个字符；  
* 从根节点到某一节点，路径上经过的字符连接起来，为该节点对应的字符串；  
* 每个节点的所有子节点包含的字符都不相同.  


----------

### （二）字典树定义与操作

**静态数组版（以一棵26个小写字母组成的Trie为例）：**  

```  
struct Trie {  
    Trie* next[26]; // 指向26个儿子节点  
    bool flag; // 从根节点到当前节点是否构成单词  
	...... // 附加相关信息  
} node[MAX_N];  

int num; // 当前分配的节点编号  
  
// 初始化  
void init () {  
    num = 0;  
    memset(node, 0, sizeof(node));  
}  

// 得到字符c相对于'a'的编号  
int getid (char c) { return c - 'a'; }  
  
// 在Trie中插入字符串s  
void insert (char* s) {  
    Trie* head = &node[0]; // node[0]为根节点  
    while (*s) { // 遍历字符串  
        int id = getid(*s++);  
        if ((head->next[id]) == NULL) // 若该儿子不存在，则分配新的节点  
            head->next[id] = &node[++num];  
        head = head->next[id]; // 指针指向儿子节点  
    }  
	head->flag = true; // 最后做上标记  
}  

// 在Trie中查找字符串s  
bool search(char* s) {  
    Trie* head = &node[0];  
    while (*s) {  
        int id = getid(*s++);  
        if (head->next[id] == NULL) return false;  
        head = head->next[id];  
    }  
    return head->flag;  
}  
```  


**动态分配版：**  


```  
struct Trie {
    Trie* next[26];
    int cnt;
    int getid(char c) { return c - 'a'; }
    Trie* newnode() {return (Trie*) calloc(1, sizeof(Trie)); }
    void insert(Trie* root, char* s) {
        Trie* head = root;
        while (*s) {
            int id = getid(*s++);
            if (head->next[id] == NULL)
                head->next[id] = newnode();
            head = head->next[id];
            (head->cnt)++;
        }
    }
    bool search(Trie* root, char* s) {
        Trie* head = root;
        while (*s) {
            int id = getid(*s++);
            if (head->next[id] == NULL) return false;
            head = head->next[id];
        }
        return head->flag;
    }
    void del(Trie* root) {
        Trie* head = root;
        for (int i = 0; i < 26; ++i)
            if (head->next[i] != NULL) del(head->next[i]);
        free(root);
    }
} trie;  
```  


----------

### （三）字典树的应用


 1) `串的快速检索`  
 Q：给出N个单词组成的熟词表，以及一篇全用小写英文书写的文章，请你按最早出现的顺序写出所有不在熟词表中的生词。  
 A： 我们可以用数组枚举，用哈希，用字典树，先把熟词建一棵树，然后读入文章进行比较，这种方法效率是比较高的。  


----------


 2) `“串”排序`  
 Q：给定N个互不相同的仅由一个单词构成的英文名，让你将他们按字典序从小到大输出。  
 A：用字典树进行排序，采用数组的方式创建字典树，这棵树的每个结点的所有儿子很显然地按照其字母大小排序。对这棵树进行先序遍历即可。  


----------


 3) `最长公共前缀`  
 A： 对所有串建立字典树，对于两个串的最长公共前缀的长度即他们所在的结点的公共祖先个数，于是，问题就转化为当时公共祖先问题。  


----------

### （四）字典树的例题  


> [HDU 1251 统计难题](http://blog.csdn.net/xmzyt1996/article/details/49407595)  
> [POJ 3630 Phone List](http://blog.csdn.net/xmzyt1996/article/details/49408063)  
> [POJ 2503 Babelfish](http://blog.csdn.net/xmzyt1996/article/details/49408175)  
> [HDU 1075 What Are You Talking About](http://blog.csdn.net/xmzyt1996/article/details/49408619)  
> [HDU 1247 Hat’s Words](http://blog.csdn.net/xmzyt1996/article/details/49409031)  
> [POJ 1056 IMMEDIATE DECODABILITY](http://blog.csdn.net/xmzyt1996/article/details/49430881)  
> [HDU 3172 Virtual Friends](http://blog.csdn.net/xmzyt1996/article/details/49431249)  
> [POJ 2001 Shortest Prefixes](http://blog.csdn.net/xmzyt1996/article/details/49431613)  
> [POJ 1204 Word Puzzles](http://blog.csdn.net/xmzyt1996/article/details/49432173)  
> [HDU 1800 Flying to the Mars](http://blog.csdn.net/xmzyt1996/article/details/49432673)  
> [HDU 1298 T9](http://blog.csdn.net/xmzyt1996/article/details/49432775)  

# Trie4j

基于AC自动机（Aho-Corasick automaton）实现的关键词、敏感词、停用词等匹配工具。

[Aho–Corasick](http://cr.yp.to/bib/1975/aho.pdf) 算法是 Alfred V. Aho 和 Margaret J. Corasick 在 1975 年发明的一种字符串搜索算法。它是一种字典匹配算法，可在输入文本中定位有限字符串集（字典）的元素。它同时匹配所有字符串。该算法的复杂性与字符串长度加上搜索文本的长度加上输出匹配的数量成线性关系。

## Usage

### 匹配所有关键词

```java
Trie trie = Trie.builder().addKeywords("雨疏", "风骤", "残酒", "卷帘人", "知否").build();
Emits emits = trie.findAll("昨夜雨疏风骤，浓睡不消残酒。试问卷帘人，却道海棠依旧。知否，知否？应是绿肥红瘦。");
```

```text
[2:4=雨疏, 4:6=风骤, 11:13=残酒, 16:19=卷帘人, 27:29=知否, 30:32=知否]
```

### 匹配首个关键词

```java
Trie trie = Trie.builder().addKeywords("雨疏", "风骤", "残酒", "卷帘人", "知否").build();
Emit emit = trie.findFirst("昨夜雨疏风骤，浓睡不消残酒。试问卷帘人，却道海棠依旧。知否，知否？应是绿肥红瘦。");
```

```text
2:4=雨疏
```

### 匹配所有关键词 忽略大小写

```java
Trie trie = Trie.builder().addKeywords("poetry", "TRANSLATION").build();
Emits emits = trie.findAllIgnoreCase("Poetry is what gets lost in translation.");
```

```text
[0:6=poetry, 28:39=TRANSLATION]
```

### 匹配首个关键词 忽略大小写

```java
Trie trie = Trie.builder().addKeywords("poetry", "TRANSLATION").build();
Emit emit = trie.findFirstIgnoreCase("Poetry is what gets lost in translation.");
```

```text
0:6=poetry
```

### 切分词

```java
Trie trie = Trie.builder().addKeywords("溪亭", "归路", "藕花", "争渡").build();
Emits emits = trie.findAllIgnoreCase("常记溪亭日暮，沉醉不知归路。兴尽晚回舟，误入藕花深处。争渡，争渡，惊起一滩鸥鹭。");
List<Token> tokens = emits.tokenize();
```

```text
["常记", "溪亭(2:4=溪亭)", "日暮，沉醉不知", "归路(11:13=归路)", "。兴尽晚回舟，误入", "藕花(22:24=藕花)", "深处。", "争渡(27:29=争渡)", "，", "争渡(30:32=争渡)", "，惊起一滩鸥鹭。"]
```

### 替换关键词

```java
Trie trie = Trie.builder().addKeywords("0元", "砍一刀", "免费拿", "免费领").build();
Emits emits = trie.findAllIgnoreCase("我正在参加砍价，砍到0元就可以免费拿啦。亲~帮我砍一刀呗，咱们一起免费领好货。");
String result1 = emits.replaceWith("*");
String result2 = emits.replaceWith("@#$%^&*");
```

```text
我正在参加砍价，砍到**就可以***啦。亲~帮我***呗，咱们一起***好货。
我正在参加砍价，砍到%^就可以#$%啦。亲~帮我%^&呗，咱们一起&*@好货。
```

## Contact

- [提交问题](https://github.com/yihleego/trie4j/issues)

## License

This project is under the Apache 2.0 license. See the [LICENSE](LICENSE) file for details.

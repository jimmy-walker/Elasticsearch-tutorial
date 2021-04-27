# 查询表达式(*Query DSL*)

## multi match

`multi_match` 查询建立在 `match` 查询之上，重要的是它允许对多个字段查询

### fields 字段

#### 通配符

`fields` 字段中的值支持通配符`*` , 设置 `mess*` 依旧可以查询出 `message` 字段中的匹配。

```json
GET multimatchtest/multimatch_test/_search
{
  "query": {
    "multi_match": {
      "query": "multimatch",
      "fields": ["subject", "mess*"]
    }
  }
}
```

#### 提升字段权重

在查询字段后使用 `^` 符号可以提高字段的权重，增加字段的分数 `_score` 。例如，我们想增加 `subject` 字段的权重。

```json
GET multimatchtest/multimatch_test/_search
{
  "query": {
    "multi_match": {
      "query": "multimatch",
      "fields": ["subject^3", "mess*"]
    }
  }
}
```

虽然文档 1 和 文档 2 中都含有相同数量的 `multimatch` 词条，但可以看出，搜索结果中 `subject` 中含有`multimatch` 的分数是另一个文档的 3 倍。

<u>J注意Boosting不仅仅意味着计算出来的分数(calculated score)直接乘以boost factor，最终的boost value会经过归一化以及其他一些内部的优化。</u>

```json
  "hits": {
    "total": 2,
    "max_score": 0.8630463,
    "hits": [
      {
        "_index": "multimatchtest",
        "_type": "multimatch_test",
        "_id": "1",
        "_score": 0.8630463,
        "_source": {
          "subject": "this is a multimatch test",
          "message": "blala blalba"
        }
      },
      {
        "_index": "multimatchtest",
        "_type": "multimatch_test",
        "_id": "2",
        "_score": 0.2876821,
        "_source": {
          "subject": "blala blalba",
          "message": "this is a multimatch test"
        }
      }
    ]
  }
}
```

如果在 `multimatch` 查询中不指定 `fields` 参数，默认会将文档中的所有字段都匹配一遍。但不建议这么做，可能会出现性能问题，也没有什么意义。

### multi_match查询的类型

`multi_match` 查询内部到底如何执行主要取决于它的 `type` 参数，这个参数的可取得值如下

- `best_fields` 是默认类型，会将任何与查询匹配的文档作为结果返回，但是只使用最佳字段的 _score 评分作为评分结果返回。
- `most_fields` 将任何与查询匹配的文档作为结果返回，并所有匹配字段的评分合并起来
- `phrase` 在 `fields` 中的每个字段上均执行 `match_phrase` 查询，并将最佳字段的 _score 作为结果返回
- `phrase_prefix` 在 `fields` 中的字段上均执行 `match_phrase_prefix` 查询，并将每个字段的分数进行合并

## 示例

![](picture/ES示例.jpg)

## 参考资料

https://www.cnblogs.com/reycg-blog/p/10055039.html
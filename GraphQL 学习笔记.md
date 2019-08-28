## GraphQL 学习笔记
### 概念
* GraphQL 是一个用于 API 的查询语言，是一个使用基于类型系统来执行查询的服务端运行时（类型系统由你的数据定义）

### 查询和变更
* **字段（Field)**：GraphQL 是关于请求对象上的特定字段。字段既能指字符串类型，也能指代对象类型。GraphQL查询能够遍历相关对象及其字段，使得客户端可以一次请求查询大量相关数据。

<pre>{
  	hero {
    name
	 #查询可以有备注！
    friends {
      name
    }
  }
}</pre>
<pre>{
  "data": {
    "hero": {
      "name": "R2-D2",
      "friends": [
        {
          "name": "Luke Skywalker"
        },
        {
          "name": "Han Solo"
        },
        {
          "name": "Leia Organa"
        }
      ]
    }
  }
}</pre>

* **参数（Arguments)** :可以给字段传递参数。也可以给[标量](https://graphql.cn/learn/schema/#scalar-types)字段传递参数，用于实现服务端的一次转换，而不用每个客户端分别转换。

<pre>{
  human(id: "1000") {
    name
    height(unit: FOOT)
  }
}</pre>

<pre>{
  "data": {
    "human": {
      "name": "Luke Skywalker",
      "height": 5.6430448
    }
  }
}
</pre>

* **别名（Aliases）** :设置别名就可以通过不同参数，来查询相同字段。如果不取别名，将会产生冲突。

<pre>{
  empireHero: hero(episode: EMPIRE) {
    name
  }
  jediHero: hero(episode: JEDI) {
    name
  }
}</pre>
<pre>{
  "data": {
    "empireHero": {
      "name": "Luke Skywalker"
    },
    "jediHero": {
      "name": "R2-D2"
    }
  }
}</pre>
* **片段（Fragments）** ：可复用单元，片段使你能够组织一组字段，然后在需要它们的的地方引入。基本语法 <pre>fragment xx on xx</pre>

<pre>{
  leftComparison: hero(episode: EMPIRE) {
    ...comparisonFields
  }
  rightComparison: hero(episode: JEDI) {
    ...comparisonFields
  }
}

fragment comparisonFields on Character {
  name
  appearsIn
  friends {
    name
  }
}</pre>
<pre>{
  "data": {
    "leftComparison": {
      "name": "Luke Skywalker",
      "appearsIn": [
        "NEWHOPE",
        "EMPIRE",
        "JEDI"
      ],
      "friends": [
        {
          "name": "Han Solo"
        },
        {
          "name": "Leia Organa"
        },
        {
          "name": "C-3PO"
        },
        {
          "name": "R2-D2"
        }
      ]
    },
    "rightComparison": {
      "name": "R2-D2",
      "appearsIn": [
        "NEWHOPE",
        "EMPIRE",
        "JEDI"
      ],
      "friends": [
        {
          "name": "Luke Skywalker"
        },
        {
          "name": "Han Solo"
        },
        {
          "name": "Leia Organa"
        }
      ]
    }
  }
}</pre>片段可以访问查询或变更中声明的变量。<pre>query HeroComparison($first: Int = 3) {
  leftComparison: hero(episode: EMPIRE) {
    ...comparisonFields
  }
  rightComparison: hero(episode: JEDI) {
    ...comparisonFields
  }
}

fragment comparisonFields on Character {
  name
  friendsConnection(first: $first) {
    totalCount
    edges {
      node {
        name
      }
    }
  }
}</pre><pre>{
  "data": {
    "leftComparison": {
      "name": "Luke Skywalker",
      "friendsConnection": {
        "totalCount": 4,
        "edges": [
          {
            "node": {
              "name": "Han Solo"
            }
          },
          {
            "node": {
              "name": "Leia Organa"
            }
          },
          {
            "node": {
              "name": "C-3PO"
            }
          }
        ]
      }
    },
    "rightComparison": {
      "name": "R2-D2",
      "friendsConnection": {
        "totalCount": 3,
        "edges": [
          {
            "node": {
              "name": "Luke Skywalker"
            }
          },
          {
            "node": {
              "name": "Han Solo"
            }
          },
          {
            "node": {
              "name": "Leia Organa"
            }
          }
        ]
      }
    }
  }
}</pre>

* **操作名称（Operation name)**
 * **操作类型**：可以是 query、mutation 或 subscription。描述你打算做什么类型的操作。操作类型是必需的，除非你使用查询简写语法，在这种情况下，你无法为操作提供名称或变量定义。
 * **操作名称**：是你的操作的有意义和明确的名称。它仅在有多个操作的文档中是必需的，但我们鼓励使用它，因为它对于调试和服务器端日志记录非常有用

 <pre>query HeroNameAndFriends {
  hero {
    name
    friends {
      name
    }
  }
}</pre>
* **变量（Variables）**：变量定义看上去像是上述查询中的 ($episode: Episode)。其工作方式跟类型语言中函数的参数定义一样。**它以列出所有变量，变量前缀必须为 $，后跟其类型，本例中为 Episode**。变量都必须是标量、枚举型或者输入对象类型。！代表必填的。可以设置默认变量。
 
<pre>query HeroNameAndFriends($episode: Episode = "JEDI") {
  hero(episode: $episode) {
    name
    friends {
      name
    }
  }
}</pre>

* **指令（Directives)**
 * **@include(if: Boolean)** 仅在参数为 true 时，包含此字段。
 * **@skip(if: Boolean)** 如果参数为 true，跳过此字段。
 
<pre>query Hero($episode: Episode, $withFriends: Boolean!) {
  hero(episode: $episode) {
    name
    friends @include(if: $withFriends) {
      name
    }
  }
}</pre> 
<pre>{
  "episode": "JEDI",
  "withFriends": false
}</pre>
<pre>{
  "data": {
    "hero": {
      "name": "R2-D2"
    }
  }
}</pre>
* **变更（Mutations**：写入的操作都应该显式通过变更（**mutation**）来发送。<pre>mutation CreateReviewForEpisode($ep: Episode!, $review: ReviewInput!) {
  createReview(episode: $ep, review: $review) {
    stars
    commentary
  }
}</pre>
查询变更的主要区别：**查询字段时，是并行执行，而变更字段时，是线性执行，一个接着一个。**

* **内联片段（Inline Fragments)**
如果要请求具体类型上的字段，你需要使用一个类型条件内联片段。因为第一个片段标注为 ... on Droid，primaryFunction 仅在 hero 返回的 Character 为 Droid 类型时才会执行。同理适用于 Human 类型的 height 字段。


<pre>query HeroForEpisode($ep: Episode!) {
  hero(episode: $ep) {
    name
    ... on Droid {
      primaryFunction
    }
    ... on Human {
      height
    }
  }
}</pre>
__typename，一个元字段，以获得那个位置的对象类型名称。
<pre>{
  search(text: "an") {
    __typename
    ... on Human {
      name
    }
    ... on Droid {
      name
    }
    ... on Starship {
      name
    }
  }
}</pre>
<pre>{
  "data": {
    "search": [
      {
        "__typename": "Human",
        "name": "Han Solo"
      },
      {
        "__typename": "Human",
        "name": "Leia Organa"
      },
      {
        "__typename": "Starship",
        "name": "TIE Advanced x1"
      }
    ]
  }
}
</pre>
### Schema 和类型
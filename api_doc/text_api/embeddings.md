# Embeddings 向量嵌入

获取给定输入的向量表示，可以轻松被机器学习模型和算法使用。

## 创建嵌入向量

**POST** `https://api.modelverse.cn/v1/embeddings`

创建表示输入文本的嵌入向量。

### 请求参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| input | string 或 array | 是 | 要嵌入的输入文本，编码为字符串或 token 数组。要在单个请求中嵌入多个输入，请传递字符串数组或 token 数组的数组。输入不得超过模型的最大输入 token（所有嵌入模型为 8192 个 token），不能为空字符串，任何数组必须为 2048 维或更少。单个请求中所有输入的 token 总和最多为 300,000 个。 |
| model | string | 是 | 要使用的模型 ID。您可以使用列表模型 API 查看所有可用模型，或查看我们的模型概述以获取描述。 |
| dimensions | integer | 否 | 生成的输出嵌入向量应具有的维度数。仅在 text-embedding-3 及更高版本的模型中支持。 |
| encoding_format | string | 否 | 返回嵌入向量的格式。可以是 `float` 或 `base64`。默认值：`float` |

### 请求示例

```bash
curl https://api.modelverse.cn/v1/embeddings \
  -H "Authorization: Bearer $MODELVERSE_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "input": "The food was delicious and the waiter...",
    "model": "text-embedding-3-large",
    "encoding_format": "float"
  }'
```

### 响应示例

```json
{
  "object": "list",
  "data": [
    {
      "object": "embedding",
      "embedding": [
        0.0023064255,
        -0.009327292,
        ...
        -0.0028842222
      ],
      "index": 0
    }
  ],
  "model": "text-embedding-3-large",
  "usage": {
    "prompt_tokens": 8,
    "total_tokens": 8
  }
}
```

## 嵌入对象

表示嵌入端点返回的嵌入向量。

| 字段 | 类型 | 说明 |
|------|------|------|
| embedding | array | 嵌入向量，是一个浮点数列表。向量的长度取决于模型。 |
| index | integer | 嵌入在嵌入列表中的索引。 |
| object | string | 对象类型，始终为 "embedding"。 |

## 什么是嵌入向量？

OpenAI 的文本嵌入可以衡量文本字符串的相关性。嵌入通常用于：

- **搜索**（结果按与查询字符串的相关性排序）
- **聚类**（文本字符串按相似性分组）
- **推荐**（推荐具有相关文本字符串的项目）
- **异常检测**（识别相关性较低的异常值）
- **多样性测量**（分析相似性分布）
- **分类**（文本字符串按其最相似的标签分类）

嵌入是浮点数的向量（列表）。两个向量之间的距离衡量它们的相关性。小距离表示高相关性，大距离表示低相关性。

## 如何获取嵌入向量

要获取嵌入向量，请将文本字符串发送到嵌入 API 端点以及嵌入模型名称（例如 `text-embedding-3-large`）：


### Python 示例

```python
from openai import OpenAI

client = OpenAI(api_key="YOUR_MODELVERSE_API_KEY",base_url="https://api.modelverse.cn/v1")

response = client.embeddings.create(
    input="Your text string goes here",
    model="text-embedding-3-large"
)

print(response.data[0].embedding)
```

响应包含嵌入向量（浮点数列表）以及一些附加元数据。您可以提取嵌入向量，将其保存在向量数据库中，并用于许多不同的用例。

默认情况下，`text-embedding-3-small` 的嵌入向量长度为 `1536`，`text-embedding-3-large` 为 `3072`。要在不丢失其概念表示属性的情况下减少嵌入的维度，请传入 `dimensions` 参数。

## 嵌入模型

OpenAI 提供两个强大的第三代嵌入模型（模型 ID 中用 `-3` 表示）。

使用按输入 token 定价。以下是每美元文本页数的示例（假设每页约 800 个 token）：

| 模型 | 每美元约页数 | MTEB 评估性能 | 最大输入 |
|------|-------------|--------------|---------|
| text-embedding-3-large | 9,615 | 64.6% | 8192 |
| text-embedding-ada-002 | 12,500 | 61.0% | 8192 |

## 降低嵌入维度

使用较大的嵌入向量（例如将它们存储在向量存储中进行检索）通常比使用较小的嵌入向量成本更高，并且消耗更多的计算、内存和存储。

我们的两个新嵌入模型都使用一种技术进行训练，允许开发人员权衡使用嵌入的性能和成本。具体来说，开发人员可以通过传入 `dimensions` API 参数来缩短嵌入（即从序列末尾删除一些数字），而不会丢失嵌入的概念表示属性。

例如，在 MTEB 基准测试中，`text-embedding-3-large` 嵌入可以缩短到 256 的大小，同时仍然优于大小为 1536 的未缩短 `text-embedding-ada-002` 嵌入。

### Python 示例：手动归一化维度

```python
from openai import OpenAI
import numpy as np

client = OpenAI(api_key="YOUR_MODELVERSE_API_KEY",base_url="https://api.modelverse.cn/v1")

def normalize_l2(x):
    x = np.array(x)
    if x.ndim == 1:
        norm = np.linalg.norm(x)
        if norm == 0:
            return x
        return x / norm
    else:
        norm = np.linalg.norm(x, 2, axis=1, keepdims=True)
        return np.where(norm == 0, x, x / norm)

response = client.embeddings.create(
    model="text-embedding-3-large",
    input="Testing 123",
    encoding_format="float"
)

cut_dim = response.data[0].embedding[:256]
norm_dim = normalize_l2(cut_dim)

print(norm_dim)
```

## 使用场景

### 1. 基于嵌入的搜索进行问答

将附加信息放入模型的上下文窗口中。这在许多用例中是有效的，但会导致更高的 token 成本。

```python
query = f"""使用以下关于 2022 年冬季奥运会的文章回答后续问题。如果找不到答案，请写"我不知道。"

文章：
\"\"\"
{wikipedia_article_on_curling}
\"\"\"

问题：哪些运动员在 2022 年冬季奥运会上获得了冰壶金牌？
"""

response = client.chat.completions.create(
    messages=[
        {'role': 'system', 'content': '你回答有关 2022 年冬季奥运会的问题。'},
        {'role': 'user', 'content': query},
    ],
    model=GPT_MODEL,
    temperature=0,
)

print(response.choices[0].message.content)
```

### 2. 使用嵌入的文本搜索

要检索最相关的文档，我们使用查询的嵌入向量和每个文档之间的余弦相似度，并返回得分最高的文档。

```python
from openai.embeddings_utils import get_embedding, cosine_similarity

def search_reviews(df, product_description, n=3):
    embedding = get_embedding(
        product_description,
        model='text-embedding-3-large'
    )
    df['similarities'] = df.ada_embedding.apply(
        lambda x: cosine_similarity(x, embedding)
    )
    res = df.sort_values('similarities', ascending=False).head(n)
    return res

res = search_reviews(df, 'delicious beans', n=3)
```

### 3. 代码搜索

代码搜索的工作方式类似于基于嵌入的文本搜索。我们提供了一种从给定存储库中的所有 Python 文件中提取 Python 函数的方法。然后，每个函数都由 `text-embedding-3-large` 模型索引。

```python
from openai.embeddings_utils import get_embedding, cosine_similarity

df['code_embedding'] = df['code'].apply(
    lambda x: get_embedding(x, model='text-embedding-3-large')
)

def search_functions(df, code_query, n=3):
    embedding = get_embedding(
        code_query,
        model='text-embedding-3-large'
    )
    df['similarities'] = df.code_embedding.apply(
        lambda x: cosine_similarity(x, embedding)
    )
    res = df.sort_values('similarities', ascending=False).head(n)
    return res

res = search_functions(df, 'Completions API tests', n=3)
```

### 4. 推荐系统

由于嵌入向量之间的较短距离表示更大的相似性，因此嵌入可用于推荐。

```python
def recommendations_from_strings(
    strings: List[str],
    index_of_source_string: int,
    model="text-embedding-3-large",
) -> List[int]:
    """返回给定字符串的最近邻居。"""
    # 获取所有字符串的嵌入
    embeddings = [
        embedding_from_string(string, model=model)
        for string in strings
    ]
    
    # 获取源字符串的嵌入
    query_embedding = embeddings[index_of_source_string]
    
    # 获取源嵌入和其他嵌入之间的距离
    distances = distances_from_embeddings(
        query_embedding,
        embeddings,
        distance_metric="cosine"
    )
    
    # 获取最近邻居的索引
    indices_of_nearest_neighbors = \
        indices_of_nearest_neighbors_from_distances(distances)
    
    return indices_of_nearest_neighbors
```

### 5. 聚类分析

聚类是理解大量文本数据的一种方法。嵌入对此任务很有用，因为它们提供了每个文本的语义有意义的向量表示。

```python
import numpy as np
from sklearn.cluster import KMeans

matrix = np.vstack(df.ada_embedding.values)
n_clusters = 4

kmeans = KMeans(
    n_clusters=n_clusters,
    init='k-means++',
    random_state=42
)
kmeans.fit(matrix)
df['Cluster'] = kmeans.labels_
```

### 6. 零样本分类

我们可以使用嵌入进行零样本分类，而无需任何标记的训练数据。对于每个类，我们嵌入类名或类的简短描述。

```python
from openai.embeddings_utils import cosine_similarity, get_embedding

df = df[df.Score != 3]
df['sentiment'] = df.Score.replace({
    1: 'negative',
    2: 'negative',
    4: 'positive',
    5: 'positive'
})

labels = ['negative', 'positive']
label_embeddings = [
    get_embedding(label, model=model)
    for label in labels
]

def label_score(review_embedding, label_embeddings):
    return cosine_similarity(review_embedding, label_embeddings[1]) - \
           cosine_similarity(review_embedding, label_embeddings[0])

prediction = 'positive' if label_score('Sample Review', label_embeddings) > 0 else 'negative'
```

## 常见问题

### 如何在嵌入之前知道字符串有多少个 token？

在 Python 中，您可以使用 OpenAI 的分词器 [`tiktoken`](https://github.com/openai/tiktoken) 将字符串拆分为 token。

```python
import tiktoken

def num_tokens_from_string(string: str, encoding_name: str) -> int:
    """返回文本字符串中的 token 数量。"""
    encoding = tiktoken.get_encoding(encoding_name)
    num_tokens = len(encoding.encode(string))
    return num_tokens

num_tokens_from_string("tiktoken is great!", "cl100k_base")
```

对于第三代嵌入模型（如 `text-embedding-3-large`），使用 `cl100k_base` 编码。

### 如何快速检索 K 个最近的嵌入向量？

为了快速搜索许多向量，我们建议使用向量数据库。

### 应该使用哪个距离函数？

我们推荐使用余弦相似度。距离函数的选择通常不会有太大影响。

OpenAI 嵌入被归一化为长度 1，这意味着：

- 余弦相似度可以仅使用点积计算得稍快一些
- 余弦相似度和欧几里得距离将产生相同的排名

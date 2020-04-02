# Query DSL

Elasticsearch provides a full Query DSL (Domain Specific Language) based on JSON to define queries. Think of the Query DSL as an AST (Abstract Syntax Tree) of queries, consisting of two types of clauses:

Elasticsearch 提供了基于 JSON 的完整查询 DSL（特定于域的语言）来定义查询。将查询 DSL 视为查询的 AST（抽象语法树），它由两种子句组成：

## Leaf query clauses 子查询

Leaf query clauses look for a particular value in a particular field, such as the `match`, `term` or `range` queries. These queries can be used by themselves.

叶查询子句中寻找一个特定的值在某一特定领域，如 `match`，`term` 或 `range` 查询。这些查询可以自己使用。

## Compound query clauses 复合查询

Compound query clauses wrap other leaf or compound queries and are used to combine multiple queries in a logical fashion (such as the `bool` or `dis_max` query), or to alter their behaviour (such as the `constant_score` query).

复合查询子句包装其他叶查询或复合查询，并用于以逻辑方式组合多个查询（例如 `bool` 或 `dis_max` 查询），或更改其行为（例如 `constant_score` 查询）。

Query clauses behave differently depending on whether they are used in query context or filter context.

查询子句的行为会有所不同，具体取决于它们是在 查询上下文中还是在过滤器上下文中使用。

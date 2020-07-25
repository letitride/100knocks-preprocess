列名の指定

```
df[[column1, column2, ...]]
```

条件の指定

```
df[[column1, column2, ...]].query('column1 == "value1" & column2 != "value2" & (100 <= column3 <= 200)')
```

前方一致

```
df[[column1, column2, ...]].query('column1.str.startswith("prefix")', engine="python")
```

後方一致

```
df[[column1, column2, ...]].query('column1.str.endswith("prefix")', engine="python")
```

部分一致

```
df.query('column1.str.contains("value")', engine="python")
```

正規表現での一致

```
df.query('column1.str.match("^[A-F]")', engine="python")
```

ソート

```
df.sort_values("column1", ascending=True )
```

列の値に対してランクを付与

```method=```同値に対してのランクの扱い。```min```で3位タイなど競技でよくある表現。

```ascending=```は値が低いほど順位が高いか否か。要は昇順で順位をふるか降順で順位を降るか
```
df["column1"].rank(method="min", ascending=False)
```

ランクを元のデータフレームに取り入れる

```concat```はSQLでいうところの```UNION```。```axis=0```で"行"を追加する通常のUNIONだが、```axis=1```で列を追加できる。便利。

```
new_df = pd.concat([
df[[column1, column2]],
df["column1"].rank(method="min", ascending=False)
], axis=1)

#新しいDataFrameのcolumn nameを作成
new_df.columns = ["column1", "column2", "ranking_of_column1"]
```

データフレームの行数をカウントする

```
len(df)
```

列内のユニークデータ数をカウントする
```
len(df_receipt["column1"].unique())
```

集約 集計対象のカラムは列参照するような書式で書ける。

```
df_receipt.groupby("column1")[["column2", "column3"]].sum()
```

```agg()```は1つの列に対して複数の集約、複数の列に対して別の集約を記述できる

```
df.groupby("column1").agg({"column2":["max", "min"], "column2":["mean"]})
```
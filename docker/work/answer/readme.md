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
df[[column1, column2, ...]].query('column1.str.endswith("suffix")', engine="python")
```

部分一致

```
df.query('column1.str.contains("value")', engine="python")
```

正規表現での一致

```
df.query('column1.str.match("^[A-F]")', engine="python")
```

指定列の各行が条件と一致しているか調べる (queryはdfを返すがこのパターンは行のTrue/Falseの判定のみ行う)
```
df["column1"] >= condition
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

ユニーク数のカウント

```nunique()```

頻出値

```mode()```

標本分散

```var(ddof=0)```

標本標準偏差

```std(ddof=0)```

パーセルタイル

```quantile(q=[0, 0.25,0.5,0.75,1])```

```np.quantile( df["column"] , 0.25 )```

Join

```
pd.merge(left_tb, right_tb[["fkey", "column1"]], on="fkey", how="inner")
```

重複行の確認 column1とcolumn2の値がすでに存在する場合Trueを返す

```
df.duplicated(subset=['column1', 'column2'])
```

重複行の削除は以下のように行えばよい

```
df[~df.duplicated(subset=['column1', 'column2'])]
```

行、列をずらす 時系列データに使用すると日付列と対応した値列が下方向に一つずれる

```
df.shift()
```

クロス集計

```
df_summary = pd.pivot_table(df, index="column1_for_rows", columns="column2_for_col", values="column3_for_agg", aggfunc="sum").reset_index()
```

値を置換

```
pd.replace("from_value", "to_value")
```

列データを行方向に置き換える

```df.stack()```

```
print(df)
#    A  B  C   D
# 0  a  x  1  10
# 1  a  y  2  20
# 2  a  z  3  30

s = df.stack()
print(s)
# 0  A     a
#    B     x
#    C     1
#    D    10
# 1  A     a
#    B     y
#    C     2
#    D    20
# 2  A     a
#    B     z
#    C     3
#    D    30
```

元に戻すには```unstack()```

文字列を日付型に

```
pd.to_datetime( df["str_column"])
```

unix timestampを日付型に

```
pd.to_datetime( df["unix_timestamp_column"], unit="s")
```

日付型を文字列型に

```
df["date_column"].dt.strftime("%Y%m%d")
```

mapで要素の置換 mapにdictを与えるとkeyと完全一致する文字をvalueに置換する

```
df_customer["address"].map({"from_val1":"to_val1", "from_val2":"to_val2", ...})
```

カテゴリデータの列をOne hot encoding (ダミー変数化)する

```pd.get_dummies(df[["column1", "column2"]], columns=["column2"])```

標準化

```(df.column - df.column.mean()) / df.column.std()```

標準化(scikit-lean)
```preprocessing.scale(df['column1']) ```

正規化

```preprocessing.MinMaxScaler```

常用対数 10を底とする対数

```df.column.apply(lambda x: math.log10(x))```

自然対数比

```np.log(df.column + 1)```
# my_functions
recicle_functions

### Convert long format files to wide
```py
def unstack_df(df):
    df_piv = pd.pivot(df, values='dmp_max', index=['chr','start', 'end'], columns=['query']).reset_index()
    col_df = df_piv.columns.values[:].tolist()
    return pd.DataFrame(df_piv.iloc[:, :].values, columns=col_df)
```

### Filter columns by regex
```py
def drop_columns_rex(df, string):
    droped_df = df.set_index(['seqname', 'start', 'end']).drop(df.filter(regex=string).columns, axis=1)
    return droped_df.reset_index()
```

### Drop columns by list:
```py
def drop_columns_list(df,sample_list):
    droped_df = df.set_index(['seqname', 'start', 'end']).drop(my_list, axis=1)
    return droped_df.reset_index()
```

### Include columns by list:
```py
def include_columns_list(df,sample_list):
    filter_df = df.set_index(['seqname', 'start', 'end']).filter(items=my_list, axis=1)
    return filter_df.reset_index()
```

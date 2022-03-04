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
### Read part of a file by rows and columns & filter by unique:
```py
def get_unique(file):
    file_df = pd.read_csv(file, delimiter='\t', nrows=200000, usecols=['chr','start','end','query','window'])
    if len(file_df['query'].unique()) > 1:
        unique_df = file_df.drop_duplicates(subset=['start','end','window'], keep='first')
    else:
        raise ValueError("less than one genotype in DF")
    return unique_df
```

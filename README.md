# my_functions

### Melt wide file
```py
# id_vars: columns to keep
# value_vars: columns names to use its values
# var_name: arbitrary name
# value_name arbitrary name
def melt_df(df):
    df_m = pd.melt(df, id_vars=['seqname','start','end'],
                   value_vars=df.columns[3:],
                   var_name='query',
                   value_name='variations')
    return df_m
```

### Convert long format files to wide
```py
def unstack_df(df):
    df_piv = pd.pivot(df, values='dmp_max', index=['chr','start', 'end'], columns=['query']).reset_index()
    col_df = df_piv.columns.values[:].tolist()
    return pd.DataFrame(df_piv.iloc[:, :].values, columns=col_df)
```

### Filter rows by list
```py
def select_genotypes(df,sample_list):
    df_f = df[df['query'].isin(sample_list)]
    return df_f
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
### filter rows using between
```py
down_region = 1000
up_region = 5000
def get_region(df, chromosome, down_region, up_region):
    region_df = df[(df['end'].between(down_region, up_region, inclusive="both")) & (df['chr'].str.contains(chromosome))]
    return region_df
``` 

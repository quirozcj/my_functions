# my_functions

### Map substring in a DF and insert at "2" column position
```py

def map_substring(s, dict_map):
    for key in dict_map.keys():
        if key in s:
            return dict_map[key]
        return s

names = {\
        'WhJag':'jagger',
        'WhAri':'arinalrfor',
        'Whjul':'julius',
        'Whlan':'lancer',
        'WhLan':'landmark',
        'Whmac':'mace',
        'WhSYM':'sy_mattis',
        'WhNor':'norin61',
        'Whspe':'spelta',
        'WhSta':'stanley'
        }

    new_col = df['seqname'].apply(lambda x: map_substring(x, names))
    df.insert(loc=2, column='new_names', value=new_col)
```


### Get chr size from file
```tsv
reference	chr	start	end
arinalrfor	chr1A__ari	0	602900890
arinalrfor	chr1B__ari	0	697493198
```

```py
def chr_len(file, reference, chromosome):
    len_df = pd.read_csv(file, delimiter='\t')
    ref_df = len_df[(len_df['reference'] == reference) & len_df['chr'].str.contains(f'{chromosome}')].iloc[0][3]
    return ref_df
 
 chr_len = chr_len('chr_lenghts.tsv', 'arinalrfor', 'chr7A'
```

### Make bed windows

```py
from itertools import count
window_size = 1000000
chr_length = 676644357
chr_id='chr4A__ari'

def make_windows(length, size, chromosome):
    start=[]
    w_pos=1
    for i in count(w_pos, size):
        if i > length:
            break
        else:
            start.append(w_pos)
            w_pos += size
    df = pd.DataFrame(start, columns=['start'])
    df['end'] = df['start'] + (size-1)
    df.insert(0, 'chr',chromosome)
    return df
    
my_bed = make_windows(chr_length, window_size, chr_id)
```


### Filter column by any value

```py
def filter_maximum(df, value):
    df_idx = region_df.set_index(['seqname','start','end'])
    df_max = df_idx.loc[:, (df_idx < value).any()].reset_index()
    return df_max

filter_df = filter_maximum(region_df, 30)
```

### File to list
```py
def file_to_list(file):
    file_l = pd.read_csv(file)['genotype'].tolist()
    return file_l
```

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

### Filter columns by list and transpose back:
```py
def select_genotypes(df, sample_list):
    df_f = df.T.filter(items=sample_list, axis=1).T
    return df_f
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

# my_functions
recicle_functions

### convert long format files to wide
```py
def unstack_df(df):
    df_piv = pd.pivot(df,
        values='dmp_max',
        index=['chr','start', 'end'],
        columns=['query']).reset_index()
        col_df = df_piv.columns.values[:].tolist()
    return pd.DataFrame(df_piv.iloc[:, :].values, columns=col_df)
    ```

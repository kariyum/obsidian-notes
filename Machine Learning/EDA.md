# Exploratory Data Analysis
```python
pd.read_csv(raw_data_file, index_col=[ts_col], parse_dates=[ts_col])
df[df.index < '2020-01-01']
df.drop_duplicates()
df.drop_duplicates()
df.head()

register_matplotlib_converters() # Addresses a warning
sns.set(rc={'figure.figsize':(16,4)})



sns.lineplot(data=df, x=df.index, y=df[target], hue=df['day_type']).set_title('Total Rides by Day Type')
```
![[Pasted image 20220922133639.png]]

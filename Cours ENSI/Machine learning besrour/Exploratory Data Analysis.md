# Exploring the Data
### DataFrame shape
![[Pasted image 20221002151026.png]]

```python
df.shape
```
![[Pasted image 20221002150944.png]]
### Unique values count
```python
df.nunique()
```
![[Pasted image 20221002150743.png]]

### Unique values of column
```
df['column_name'].unique()
```
![[Pasted image 20221002150850.png]]

### Null values
```python
df.isnull().sum()
```
![[Pasted image 20221002151304.png]]
>Check for unique values as well, they might be filled with unmeaningful values.

### Describe data
```python
df.describe()
```
![[Pasted image 20221002151729.png]]
>std = Standard deviation

### Check for outliers

# Relationship analysis
### Correlation matrix
```python
plt.figure(figsize=(16,6))
heatmap = sns.heatmap(df2.corr(), vmin = -1, vmax = 1, annot=True)
heatmap.set_title('Correlation Heatmap')
```
![[Pasted image 20221002155651.png]]
```python
# plt.figure(figsize=(8, 12))
heatmap = sns.heatmap(df2.corr()[['selling_price']].drop('selling_price').sort_values(by='selling_price', ascending=False), vmin=-1, vmax=1, annot=True)
heatmap.set_title("Feature correlation with selling price")
```
![[Pasted image 20221002155718.png]]
>Not here that we removed the selling_price to selling_price correlation
### Scatter plot
```python
sns.pairplot(df2)
```
![[Pasted image 20221002212600.png]]

### Relation plot
```python
sns.relplot(y='selling_price', x='km_driven', hue='fuel', data=df2)
```
![[Pasted image 20221003121944.png]]
### Distribution shape, skewness
```python
for column in df2:
    if (df2[column].dtype == "object"):
        continue
    sns.distplot(df2[column]/1000, hist_kws=dict(color='plum', edgecolor="k", linewidth=1))
    plt.show()
```
![[Pasted image 20221003122035.png]]

### Scatter plot
```python
# to make plotting easier we will remove our target variable price from the main dataframe and save it on a separate one:  
target = df.price  
df = df.drop(columns=['price'])# we use a for loop to plot our independent variables against our dependent one:  
for col in df:  
    sns.regplot(x=df[col], y=target, data=df, label=col)  
    plt.ylabel('Price')  
    plt.xlabel('')  
    plt.legend()  
    plt.tight_layout()  
    plt.show()
```
![[Pasted image 20221003123016.png]]
### Boxplot
Used to detect the outliers clearly
```python
plt.figure(figsize=(12, 4))
sns.boxplot(df['selling_price'])
plt.show()
```
![[Pasted image 20221003123435.png]]
![[Pasted image 20221003123645.png]]
>A lot of outliers above 1M

### Violin plot
```python
for column in df:
    if (df[column].dtype == "object"):
        continue
    sns.violinplot(x=column, data=df, color='salmon', whis=3)
    plt.tight_layout()
    plt.show()
```
![[Pasted image 20221003123524.png]]
>The long tail tells about the outliers
### Bar plot
```python
plt.figure(figsize = (20,15))
df['fuel'].value_counts().plot(kind='bar')
```
![[Pasted image 20221004110800.png]]
```python
plt.figure(figsize=(10,10))
for i in range(len(categorical_cols)):
    plt.subplot(2,2,i+1)
    sns.countplot(x=categorical_cols[i], data=df)
plt.tight_layout()
```
![[Pasted image 20221017084111.png]]

### Filter
```python
aux[(aux[num_col] < right )& (aux[num_col] > left)][num_col]
```
# Processing
#### Dropping columns
```python
df_new = df.drop(['column_name1', 'c2'], axis=1)
```

#### One hot encoding
```python
pd.get_dummies(data= df, columns=['fuel', 'seller_type', 'transmission', 'owner', 'seats']).drop(['name', 'mileage', 'engine', 'max_power', 'torque'], axis=1)
```

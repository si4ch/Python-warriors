# Univariate-Analysis (Exercise 8 solution)

### How to calculate the mean life expectancy for EUROPEan countries (2007).

```python
#1

df_gapminder[(df_gapminder['continent'] == 'Europe') & (df_gapminder['year'] == 2007)]['life_exp'].mean()
```

### Is it possible to calculate the average of the column “continent”? Why or why not?

*No, it is not possible because 'continent' is a categorical nominal variable.*

*Average (mean) can be calculated for numerical variables only.*

### - Subtract each observation in `numbers` from the `average` of this `list`. - Then calculate the **sum** of these deviations from the `average`. What is their sum?

```python
numbers = np.array([1, 2, 3, 4])
#3
average = np.mean(numbers)
deviations = average - numbers
sum_of_deviations = np.sum(deviations)
print('Average:', average)
print('Deviations:', deviations)
print('Sum of deviations:', sum_of_deviations)
```

### Is it possible to calculate the median of the column “continent”? Why or why not?

*No, it is not possible because 'continent' is a categorical nominal variable.*

*Median can be calculated for ordered data, but continents rather do not have any common order.*

### Try to change the boxplot into the violin plot (or add it). Looking at the aforementioned quantile results and the box plot, try to interpret these measures.

```python
#5
fig, ax = plt.subplots(figsize=(8, 6))
sns.violinplot(data=mydata, ax=ax)

minimum = np.min(mydata)
q1 = np.percentile(mydata, 25)
median = np.median(mydata)
q3 = np.percentile(mydata, 75)
maximum = np.max(mydata)
mean = np.mean(mydata)

ax.scatter(0, minimum, color='red', label='Min', zorder=5)
ax.scatter(0, q1, color='orange', label='Q1', zorder=5)
ax.scatter(0, median, color='lightgreen', label='Median', zorder=5)
ax.scatter(0, q3, color='purple', label='Q3', zorder=5)
ax.scatter(0, maximum, color='yellow', label='Max', zorder=5)
ax.scatter(0, mean, color='pink', marker='D', s=60, label='Mean', zorder=5)

for value, name, color in zip(
    [minimum, q1, median, mean, q3, maximum],
    ['Min', 'Q1', 'Median', 'Mean', 'Q3', 'Max'],
    ['red', 'orange', 'lightgreen', 'pink', 'purple', 'yellow']
):
    ax.text(0.1, value, f'{name}: {value:.2f}', verticalalignment='center', color=color)

ax.set_title("Violin plot of mydata")
ax.legend()

plt.show()

print("Minimum:", minimum)
print("Q1:", q1)
print("Median:", median)
print("Q3:", q3)
print("Maximum:", maximum)
print("Mean:", mean)
print("IQR:", q3 - q1)

# Interpretation:
# The median is 12, which means that half of the values are below or equal to 12
# and half of the values are above or equal to 12.
# Q1 is 7, so about 25% of the values are below or equal to 7.
# Q3 is 14, so about 75% of the values are below or equal to 14.
# The interquartile range is Q3 - Q1 = 7, so the middle 50% of values
# are between 7 and 14.
# The boxplot does not show any clear outliers.
# The violin plot shows the shape of the distribution and where values
# are more concentrated.
```

### Calculate STD and CV for the SPEED of LEGENDARY and NOT LEGENDARY pokemons. What is the IQR deviation?

```python
#6
for leg, group in df_pokemon.groupby('Legendary')['Speed']:
    std = group.std()
    cv = (std / group.mean()) * 100
    iqr_dev = (group.quantile(0.75) - group.quantile(0.25)) / 2
    print(f"Legendary: {leg} | STD: {std}, CV: {cv}%, IQR Dev: {iqr_dev}")
```

### Try to interpret the above-mentioned result and calculate example slant ratios for several groups of Pokémon.

```python
#7
skewness = df_pokemon.groupby('Legendary')['Speed'].skew()
print(skewness)

#Interpretation: 
#Values > 0 mean the distribution is right-skewed
#Values < 0 mean the distribution is left-skewed
#Values near 0 mean it is symmetric
```

### Try to calculate the IQR Skewness coefficient for the sample data:

```python
mydata = [3, 7, 8, 5, 12, 14, 21, 13, 18]
#8

q1, median, q3 = np.percentile(mydata, [25, 50, 75])

iqr_skewness = ((q3 - median) - (median - q1)) / (q3 - q1)
print(iqr_skewness)
```

### Try to calculate the IQR Kurtosis coefficient for the sample data:

```python
mydata = [3, 7, 8, 5, 12, 14, 21, 13, 18]
#9
q1 = np.percentile(mydata, 25)
q3 = np.percentile(mydata, 75)
c10 = np.percentile(mydata, 10)
c90 = np.percentile(mydata, 90)

iqr_kurtosis = (q3 - q1) / (2 * (c90 - c10))
print(iqr_kurtosis)
```

### Add some cross-sectional plots and try to interpret the results.

```python
#10
sns.boxplot(data=df_pokemon, x='Legendary', y='Attack', hue='Legendary')
plt.title('Boxplot: Attack by Legendary Status')
plt.show()

sns.violinplot(data=df_pokemon, x='Legendary', y='Attack', hue='Legendary')
plt.title('Violin Plot: Attack by Legendary Status')
plt.show()

#Legendary pokemon have a significantly higher median attack
#The non-legendary group has several high-value outliers stretching up to 185
#Legendary group shows no outliers meaning their high attack values are standard for their group distribution
#Violin plot reveals that non-legendary attack stats are heavily concentrated in the lower range (50-70) and right-skewed 
#Legendary attack stats have a wider flatter distribution centered much higher (100-130)
```
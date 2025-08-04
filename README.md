# Diwali_slale
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
dataframe = pd.read_csv("Zomato data.csv", delimiter=",")  # Ensure delimiter is correct
print("First 5 rows of the dataset:")
print(dataframe.head())

print("\nDataset Information:")
print(dataframe.info())
if 'rate' in dataframe.columns:
    def handle_rate(value):
        try:
            value = str(value).split('/')[0]  # Extract numeric part before '/'
            return float(value) if value.replace('.', '', 1).isdigit() else None
        except (ValueError, AttributeError, TypeError):
            return None

    dataframe['rate'] = dataframe['rate'].apply(handle_rate)

    print("\nData after processing 'rate' column:")
    print(dataframe[['rate']].head())  # Show only the 'rate' column for clarity
else:
    print("\nColumn 'rate' not found in the dataset. Check CSV file.")
    print("\nUpdated DataFrame Info:")
print(dataframe.info())

print("\nFirst few rows of the cleaned dataset:")
print(dataframe.head())
dataframe.to_csv("Cleaned_Zomato_data.csv", index=False)
sns.countplot(x=dataframe['listed_in(type)'])
plt.xlabel("type of resturant")
dataframe.head()
grouped_data = dataframe.groupby('listed_in(type)')['votes'].sum()
result = pd.DataFrame({'votes': grouped_data})
plt.figure(figsize=(10, 5))  # Set figure size
plt.plot(result.index, result['votes'], color="green", marker="o", linestyle="-")  
plt.xlabel("Types of Restaurant", color="red", fontsize=15)
plt.ylabel("Votes", color="red", fontsize=15)
plt.title("Total Votes by Restaurant Type", fontsize=18)
plt.xticks(rotation=45)
plt.show()
dataframe.head()
plt.hist(dataframe['rate'],bins=5)
plt.title("rating distribution")
plt.show()
dataframe.head()
couple_data=dataframe['approx_cost(for two people)']
sns.countplot(x=couple_data)
dataframe.head()
plt.figure(figsize=(6,6))  # Set figure size
# Create a boxplot
sns.boxplot(x='online_order', y='rate', data=dataframe)
# Labeling
plt.xlabel("Online Order Availability", color="blue", fontsize=14)
plt.ylabel("Restaurant Rating", color="blue", fontsize=14)
plt.title("Boxplot of Ratings by Online Order Availability", fontsize=16)

# Show the plot
plt.show()
# Ensure column names are correct
if 'listed_in(type)' in dataframe.columns and 'online_order' in dataframe.columns:
# Create a pivot table
    pivot_table = dataframe.pivot_table(
        index='listed_in(type)', 
        columns='online_order', 
        aggfunc='size', 
        fill_value=0
    )
# Plot heatmap
    plt.figure(figsize=(8,6))
    sns.heatmap(pivot_table, annot=True, cmap='YlGnBu', fmt='d')

    # Labels and title
    plt.title("Heatmap of Online Orders by Restaurant Type", fontsize=14)
    plt.xlabel("Online Order Availability", fontsize=12, color="blue")
    plt.ylabel("Restaurant Type", fontsize=12, color="blue")

    # Show plot
    plt.show()

else:
    print("One or both of the required columns ('listed_in(type)', 'online_order') are missing.")
    
